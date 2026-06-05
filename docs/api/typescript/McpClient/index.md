Defined in: [src/mcp.ts:122](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L122)

MCP Client for interacting with Model Context Protocol servers.

## Constructors

### Constructor

```ts
new McpClient(args): McpClient;
```

Defined in: [src/mcp.ts:144](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L144)

#### Parameters

| Parameter | Type |
| --- | --- |
| `args` | [`McpClientConfig`](/docs/api/typescript/McpClientConfig/index.md) |

#### Returns

`McpClient`

## Properties

### DEFAULT\_TTL

```ts
readonly static DEFAULT_TTL: 60000 = 60000;
```

Defined in: [src/mcp.ts:124](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L124)

Default TTL for task polling in milliseconds (60 seconds).

---

### DEFAULT\_POLL\_TIMEOUT

```ts
readonly static DEFAULT_POLL_TIMEOUT: 300000 = 300000;
```

Defined in: [src/mcp.ts:127](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L127)

Default poll timeout for task completion in milliseconds (5 minutes).

## Accessors

### client

#### Get Signature

```ts
get client(): Client;
```

Defined in: [src/mcp.ts:213](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L213)

##### Returns

`Client`

---

### serverCapabilities

#### Get Signature

```ts
get serverCapabilities(): any;
```

Defined in: [src/mcp.ts:217](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L217)

##### Returns

`any`

---

### serverVersion

#### Get Signature

```ts
get serverVersion(): any;
```

Defined in: [src/mcp.ts:221](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L221)

##### Returns

`any`

---

### serverInstructions

#### Get Signature

```ts
get serverInstructions(): string;
```

Defined in: [src/mcp.ts:225](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L225)

##### Returns

`string`

---

### connectionState

#### Get Signature

```ts
get connectionState(): McpConnectionState;
```

Defined in: [src/mcp.ts:229](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L229)

##### Returns

[`McpConnectionState`](/docs/api/typescript/McpConnectionState/index.md)

---

### clientName

#### Get Signature

```ts
get clientName(): string;
```

Defined in: [src/mcp.ts:233](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L233)

##### Returns

`string`

---

### continueOnError

#### Get Signature

```ts
get continueOnError(): boolean;
```

Defined in: [src/mcp.ts:237](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L237)

##### Returns

`boolean`

---

### onToolsChanged

#### Set Signature

```ts
set onToolsChanged(callback): void;
```

Defined in: [src/mcp.ts:339](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L339)

Sets a callback invoked when the MCP server’s tool list changes at runtime.

##### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `callback` | (`oldTools`, `newTools`) => `void` | Handler receiving the previous tool names and the refreshed tool instances, or undefined to remove the callback. |

##### Returns

`void`

## Methods

### connect()

```ts
connect(reconnect?): Promise<void>;
```

Defined in: [src/mcp.ts:251](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L251)

Connects the MCP client to the server.

Called lazily before any operation that requires a connection. When `continueOnError` is true, connection failures are swallowed and the client enters a `'failed'` state — subsequent calls are no-ops until `connect(true)` is called explicitly to retry.

#### Parameters

| Parameter | Type | Default value | Description |
| --- | --- | --- | --- |
| `reconnect` | `boolean` | `false` | When true, forces a reconnect even if already connected or failed. |

#### Returns

`Promise`<`void`\>

A promise that resolves when the connection is established.

---

### disconnect()

```ts
disconnect(): Promise<void>;
```

Defined in: [src/mcp.ts:283](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L283)

Disconnects the MCP client from the server and cleans up resources.

#### Returns

`Promise`<`void`\>

A promise that resolves when the disconnection is complete.

---

### \[asyncDispose\]()

```ts
asyncDispose: Promise<void>;
```

Defined in: [src/mcp.ts:294](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L294)

Enables the `await using` pattern for automatic resource cleanup. Delegates to [McpClient.disconnect](#disconnect).

#### Returns

`Promise`<`void`\>

---

### listTools()

```ts
listTools(): Promise<McpTool[]>;
```

Defined in: [src/mcp.ts:303](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L303)

Lists the tools available on the server and returns them as executable McpTool instances.

#### Returns

`Promise`<`McpTool`\[\]>

A promise that resolves with an array of McpTool instances.

---

### callTool()

```ts
callTool(
   tool,
   args,
options?): Promise<JSONValue>;
```

Defined in: [src/mcp.ts:377](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L377)

Invoke a tool on the connected MCP server using an McpTool instance.

When `tasksConfig` was provided to the client constructor, uses experimental task-based invocation which supports long-running tools with progress tracking. Otherwise, calls tools directly without task management.

#### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `tool` | `McpTool` | The McpTool instance to invoke. |
| `args` | [`JSONValue`](/docs/api/typescript/JSONValue/index.md) | The arguments to pass to the tool. |
| `options?` | [`McpCallToolOptions`](/docs/api/typescript/McpCallToolOptions/index.md) | Optional settings for the request. |

#### Returns

`Promise`<[`JSONValue`](/docs/api/typescript/JSONValue/index.md)\>

A promise that resolves with the result of the tool invocation.