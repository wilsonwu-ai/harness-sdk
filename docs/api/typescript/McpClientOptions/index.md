Defined in: [src/mcp.ts:78](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L78)

Behavioral options shared by all MCP client configurations.

## Extends

-   `RuntimeConfig`

## Properties

### applicationName?

```ts
optional applicationName?: string;
```

Defined in: [src/mcp.ts:32](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L32)

#### Inherited from

```ts
RuntimeConfig.applicationName
```

---

### applicationVersion?

```ts
optional applicationVersion?: string;
```

Defined in: [src/mcp.ts:33](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L33)

#### Inherited from

```ts
RuntimeConfig.applicationVersion
```

---

### disableMcpInstrumentation?

```ts
optional disableMcpInstrumentation?: boolean;
```

Defined in: [src/mcp.ts:80](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L80)

Disable OpenTelemetry MCP instrumentation.

---

### tasksConfig?

```ts
optional tasksConfig?: TasksConfig;
```

Defined in: [src/mcp.ts:87](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L87)

Configuration for task-augmented tool execution (experimental). When provided (even as empty object), enables MCP task-based tool invocation. When undefined, tools are called directly without task management.

---

### elicitationCallback?

```ts
optional elicitationCallback?: ElicitationCallback;
```

Defined in: [src/mcp.ts:94](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L94)

Callback to handle server-initiated elicitation requests. When provided, the client advertises elicitation support (form + url modes) and routes incoming elicitation requests to this callback.

---

### continueOnError?

```ts
optional continueOnError?: boolean;
```

Defined in: [src/mcp.ts:97](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L97)

When true, connection failures are logged as warnings instead of throwing.

---

### logHandler?

```ts
optional logHandler?: (params) => void;
```

Defined in: [src/mcp.ts:100](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L100)

Called when the server emits a log message. Defaults to routing through the Strands logger.

#### Parameters

| Parameter | Type |
| --- | --- |
| `params` | `LoggingMessageNotificationParams` |

#### Returns

`void`