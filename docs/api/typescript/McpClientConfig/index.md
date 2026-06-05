```ts
type McpClientConfig = McpClientOptions & {
  transport?: McpTransport;
  url?: string | URL;
  auth?: McpClientCredentials;
  authProvider?: OAuthClientProvider;
  headers?: Record<string, string>;
};
```

Defined in: [src/mcp.ts:104](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L104)

Arguments for configuring an MCP Client.

## Type Declaration

| Name | Type | Description | Defined in |
| --- | --- | --- | --- |
| `transport?` | [`McpTransport`](/docs/api/typescript/McpTransport/index.md) | Pre-constructed transport. Mutually exclusive with `url`. | [src/mcp.ts:106](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L106) |
| `url?` | `string` | `URL` | Server URL. When provided, a StreamableHTTP transport is constructed automatically. | [src/mcp.ts:109](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L109) |
| `auth?` | [`McpClientCredentials`](/docs/api/typescript/McpClientCredentials/index.md) | Client credentials for OAuth machine-to-machine auth. Requires `url`. | [src/mcp.ts:112](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L112) |
| `authProvider?` | `OAuthClientProvider` | Custom OAuth provider for advanced auth flows. Requires `url`. Mutually exclusive with `auth`. | [src/mcp.ts:115](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L115) |
| `headers?` | `Record`<`string`, `string`\> | Custom headers to include on every request to the server. Requires `url`. | [src/mcp.ts:118](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/mcp.ts#L118) |