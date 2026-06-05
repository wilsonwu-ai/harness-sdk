Interventions are a composable control layer for agents. They provide a typed action model for common control concerns — authorization, guardrails, steering, and content transformation — with ordered evaluation and short-circuiting. Unlike raw [hooks](/docs/user-guide/concepts/agents/hooks/index.md) and [plugins](/docs/user-guide/concepts/plugins/index.md) which mutate event objects directly, intervention handlers return typed decisions (`proceed`, `deny`, `guide`, `confirm`, `transform`) that the framework applies with well-defined semantics — enabling automatic short-circuiting, feedback accumulation, and conflict resolution.

## Basic Usage

Create an intervention handler by extending `InterventionHandler` and overriding the lifecycle methods you need. Register handlers via the `interventions` option in agent configuration:

```typescript
class ToolGuard extends InterventionHandler {
  readonly name = 'tool-guard'
  private blockedTools: string[]

  constructor(blockedTools: string[]) {
    super()
    this.blockedTools = blockedTools
  }

  override beforeToolCall(event: BeforeToolCallEvent) {
    if (this.blockedTools.includes(event.toolUse.name)) {
      return InterventionActions.deny(
        `Tool '${event.toolUse.name}' is not allowed in this environment`
      )
    }
    return InterventionActions.proceed()
  }
}

const agent = new Agent({
  tools: [searchTool, deleteTool],
  interventions: [new ToolGuard(['delete_file'])],
})

// The agent can search freely, but any attempt to call delete_file
// is blocked before execution — the model sees the denial reason
// and adjusts its approach
await agent.invoke('Clean up the temp directory')
```

Handlers only need to override the lifecycle methods relevant to their concern — all methods default to `proceed()`.

## Action Types

Each lifecycle method returns one of five typed actions:

| Action | Factory | Description |
| --- | --- | --- |
| Proceed | `InterventionActions.proceed()` | Allow the operation to continue unchanged |
| Deny | `InterventionActions.deny(reason)` | Block the operation. Short-circuits remaining handlers |
| Guide | `InterventionActions.guide(feedback)` | Cancel and provide feedback for the model to retry with |
| Confirm | `InterventionActions.confirm(prompt)` | Pause for human approval |
| Transform | `InterventionActions.transform(apply)` | Modify event content in-place before execution continues |

The following examples show each action type in a realistic handler:

```typescript
// deny — block tool calls that access production resources
class EnvironmentGuard extends InterventionHandler {
  readonly name = 'environment-guard'

  override beforeToolCall(event: BeforeToolCallEvent) {
    const input = event.toolUse.input as Record<string, string>
    if (input.database?.includes('prod')) {
      return InterventionActions.deny('Production database access is not allowed')
    }
    return InterventionActions.proceed()
  }
}

// guide — steer the model when it tries to send emails without a subject
class EmailValidator extends InterventionHandler {
  readonly name = 'email-validator'

  override beforeToolCall(event: BeforeToolCallEvent) {
    if (event.toolUse.name === 'send_email') {
      const input = event.toolUse.input as Record<string, string>
      if (!input.subject) {
        return InterventionActions.guide('All emails must include a subject line.')
      }
    }
    return InterventionActions.proceed()
  }
}

// confirm — require human approval before deleting files
class DeleteApproval extends InterventionHandler {
  readonly name = 'delete-approval'

  override beforeToolCall(event: BeforeToolCallEvent) {
    if (event.toolUse.name === 'delete_file') {
      const input = event.toolUse.input as Record<string, string>
      return InterventionActions.confirm(
        `Approve deleting "${input.path}"?`
      )
    }
    return InterventionActions.proceed()
  }
}

// transform — redact PII from outgoing email bodies
class PiiRedactor extends InterventionHandler {
  readonly name = 'pii-redactor'

  override beforeToolCall(event: BeforeToolCallEvent) {
    if (event.toolUse.name === 'send_email') {
      return InterventionActions.transform((e) => {
        const toolEvent = e as BeforeToolCallEvent
        const input = toolEvent.toolUse.input as Record<string, string>
        input.body = input.body.replace(/\b\d{3}-\d{2}-\d{4}\b/g, '[REDACTED]')
      })
    }
    return InterventionActions.proceed()
  }
}
```

## Lifecycle Methods

Intervention handlers can override five lifecycle methods. Each method supports a specific subset of actions:

| Method | Valid Actions | When it Runs |
| --- | --- | --- |
| `beforeInvocation` | Proceed, Deny, Guide, Transform | Before the agent loop starts |
| `beforeToolCall` | Proceed, Deny, Guide, Confirm, Transform | Before each tool execution |
| `afterToolCall` | Proceed, Transform | After each tool execution |
| `beforeModelCall` | Proceed, Deny, Guide, Transform | Before each model API call |
| `afterModelCall` | Proceed, Guide, Transform | After each model response |

How actions behave depends on the lifecycle method:

| Action | Before events | After events |
| --- | --- | --- |
| **Deny** | Sets `event.cancel`, short-circuits remaining handlers | No effect (warns at runtime) |
| **Guide** | On `beforeToolCall`/`beforeInvocation`: cancels with accumulated feedback. On `beforeModelCall`: injects feedback as user message | Injects feedback and retries |
| **Confirm** | Pauses agent via interrupt/resume for human approval; denied responses set `event.cancel` | Not supported |
| **Transform** | Calls `action.apply(event)` — later handlers see modified content | Calls `action.apply(event)` |

## Evaluation Order and Short-Circuiting

Handlers evaluate in **registration order**. If any handler returns `deny()`, remaining handlers are skipped — the operation is blocked immediately. This enables efficient pipelines where fast checks (like authorization) run first and prevent expensive evaluations (like LLM-based steering) from running unnecessarily.

```typescript
class RateLimiter extends InterventionHandler {
  readonly name = 'rate-limiter'
  private callCount = 0

  override beforeToolCall(event: BeforeToolCallEvent) {
    this.callCount++
    if (this.callCount > 10) {
      // deny() short-circuits: handlers registered after this one are skipped
      return InterventionActions.deny('Rate limit exceeded')
    }
    return InterventionActions.proceed()
  }
}

class ToneSteeringHandler extends InterventionHandler {
  readonly name = 'tone-steering'

  override afterModelCall(event: AfterModelCallEvent) {
    // This handler never runs for denied tool calls
    return InterventionActions.guide('Use a more professional tone.')
  }
}

// Handlers evaluate in registration order
const agent = new Agent({
  tools: [searchTool],
  interventions: [
    new RateLimiter(),         // Evaluates first
    new ToneSteeringHandler(), // Skipped if RateLimiter denies
  ],
})
```

For `guide()` actions, all handlers continue to run and their feedback is accumulated — the model receives combined guidance from all guiding handlers.

## Error Handling

The `onError` property controls what happens when a handler throws an exception:

| Value | Behavior |
| --- | --- |
| `'throw'` | Rethrow the error (default). The invocation fails. |
| `'proceed'` | Log the error and continue as if `proceed()` was returned. |
| `'deny'` | Log the error and treat it as a `deny()` (fail-closed). |

```typescript
// 'proceed' — if this handler throws, continue as if proceed() was returned
class BestEffortLogger extends InterventionHandler {
  readonly name = 'best-effort-logger'
  readonly onError: OnError = 'proceed'

  override beforeToolCall(event: BeforeToolCallEvent) {
    // If the logging service is unreachable, the agent continues normally
    console.log(`Tool called: ${event.toolUse.name}`)
    return InterventionActions.proceed()
  }
}

// 'deny' — if this handler throws, treat it as a deny (fail-closed)
class StrictAuth extends InterventionHandler {
  readonly name = 'strict-auth'
  readonly onError: OnError = 'deny'

  override beforeToolCall(event: BeforeToolCallEvent) {
    // If the auth service is down (throws), the operation is denied
    if (!this.checkPermission(event.toolUse.name)) {
      return InterventionActions.deny('Unauthorized')
    }
    return InterventionActions.proceed()
  }

  private checkPermission(toolName: string): boolean {
    // ... call external auth service
    return true
  }
}

// 'throw' (default) — errors propagate and fail the invocation
class CriticalValidator extends InterventionHandler {
  readonly name = 'critical-validator'
  // onError defaults to 'throw'

  override beforeToolCall(event: BeforeToolCallEvent) {
    // If this throws, the entire invocation fails
    return InterventionActions.proceed()
  }
}
```

Use `'deny'` for security-critical handlers where a failure should block execution. Use `'proceed'` for non-critical handlers like logging where availability is more important than enforcement.

## Relationship to Hooks and Plugins

Interventions are built on top of the [hooks](/docs/user-guide/concepts/agents/hooks/index.md) system — under the hood, each lifecycle method registers a hook callback. The difference is in how they communicate with the framework.

[Hooks](/docs/user-guide/concepts/agents/hooks/index.md) and [plugins](/docs/user-guide/concepts/plugins/index.md) mutate event properties directly (e.g., setting `event.cancel = "reason"`). The framework doesn’t know *why* something was cancelled — was it a hard authorization denial or soft guidance to retry differently? Multiple plugins modifying the same event can conflict silently with last-write-wins semantics.

Interventions return typed actions that the framework interprets. This enables:

-   **Short-circuiting** — a `deny()` from an authorization handler skips all remaining handlers automatically. With hooks, each plugin must independently check `event.cancel` before doing work.
-   **Feedback accumulation** — multiple handlers can return `guide()` and their feedback is combined into a single message to the model, rather than overwriting each other.
-   **Human-in-the-loop** — `confirm()` integrates with the SDK’s interrupt/resume system to pause for approval without the handler needing to manage interrupt lifecycle.
-   **Ordered evaluation** — handlers always run in registration order with well-defined precedence (deny > confirm > guide > transform > proceed).
-   **Error policies** — each handler declares its own failure mode via `onError`. A logging handler can use `'proceed'` (skip on failure), while an auth handler can use `'deny'` (fail closed). Hooks have no equivalent — a thrown error always propagates.

## Related topics

-   [Human in the Loop](/docs/user-guide/concepts/agents/human-in-the-loop/index.md) — Ready-to-use intervention handler for tool approval workflows
-   [Hooks](/docs/user-guide/concepts/agents/hooks/index.md) — Low-level event callbacks for observing and modifying agent behavior
-   [Plugins](/docs/user-guide/concepts/plugins/index.md) — Bundle related hooks and tools into reusable modules
-   [Interrupts](/docs/user-guide/concepts/interrupts/index.md) — The interrupt/resume system that `confirm()` builds on

## Related pages

- [Agent Loop](/docs/user-guide/concepts/agents/agent-loop/index.md) (3 shared tags)
- [Hooks](/docs/user-guide/concepts/agents/hooks/index.md) (3 shared tags)
- [Interrupts](/docs/user-guide/concepts/interrupts/index.md) (3 shared tags)
- [Human in the Loop](/docs/user-guide/concepts/agents/human-in-the-loop/index.md) (2 shared tags)
- [Plugins](/docs/user-guide/concepts/plugins/index.md) (2 shared tags)
- [Tool Executors](/docs/user-guide/concepts/tools/executors/index.md) (2 shared tags)
- [Creating a Custom Model Provider](/docs/user-guide/concepts/model-providers/custom_model_provider/index.md) (1 shared tag)
- [Retry Strategies](/docs/user-guide/concepts/agents/retry-strategies/index.md) (1 shared tag)
- [Bidirectional Streaming Hooks](/docs/user-guide/concepts/bidirectional-streaming/hooks/index.md) (1 shared tag)
- [Steering](/docs/user-guide/concepts/plugins/steering/index.md) (1 shared tag)
