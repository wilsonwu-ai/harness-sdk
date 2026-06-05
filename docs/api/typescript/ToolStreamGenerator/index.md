```ts
type ToolStreamGenerator = AsyncGenerator<ToolStreamEvent, ToolResultBlock, undefined>;
```

Defined in: [src/tools/tool.ts:93](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/tools/tool.ts#L93)

Type alias for the async generator returned by tool stream methods. Yields ToolStreamEvents during execution and returns a ToolResultBlock.