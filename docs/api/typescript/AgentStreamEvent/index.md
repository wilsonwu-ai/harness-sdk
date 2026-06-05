```ts
type AgentStreamEvent =
  | ModelStreamUpdateEvent
  | ContentBlockEvent
  | ModelMessageEvent
  | ToolStreamUpdateEvent
  | ToolResultEvent
  | BeforeInvocationEvent
  | AfterInvocationEvent
  | BeforeModelCallEvent
  | AfterModelCallEvent
  | BeforeToolsEvent
  | AfterToolsEvent
  | BeforeToolCallEvent
  | AfterToolCallEvent
  | MessageAddedEvent
  | InterruptEvent
  | AgentResultEvent;
```

Defined in: [src/types/agent.ts:519](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/agent.ts#L519)

Union type representing all possible streaming events from an agent. This includes model events, tool events, and agent-specific lifecycle events.

This is a discriminated union where each event has a unique type field, allowing for type-safe event handling using switch statements.

Every member extends [HookableEvent](/docs/api/typescript/HookableEvent/index.md) (which extends [StreamEvent](/docs/api/typescript/StreamEvent/index.md)), making all events both streamable and subscribable via hook callbacks. Raw data objects from lower layers (model, tools) should be wrapped in a StreamEvent subclass at the agent boundary rather than added directly.