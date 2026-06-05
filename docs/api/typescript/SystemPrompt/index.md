```ts
type SystemPrompt = string | SystemContentBlock[];
```

Defined in: [src/types/messages.ts:704](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/messages.ts#L704)

System prompt for guiding model behavior. Can be a simple string or an array of content blocks for advanced caching.

## Example

```typescript
// Simple string
const prompt: SystemPrompt = 'You are a helpful assistant'

// Array with cache points for advanced caching
const prompt: SystemPrompt = [
  { textBlock: new TextBlock('You are a helpful assistant') },
  { textBlock: new TextBlock(largeContextDocument) },
  { cachePointBlock: new CachePointBlock({ cacheType: 'default' }) }
]
```