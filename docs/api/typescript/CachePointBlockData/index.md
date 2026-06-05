Defined in: [src/types/messages.ts:536](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/messages.ts#L536)

Data for a cache point block.

## Properties

### cacheType

```ts
cacheType: "default";
```

Defined in: [src/types/messages.ts:540](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/messages.ts#L540)

The cache type. Currently only ‘default’ is supported.

---

### ttl?

```ts
optional ttl?: string;
```

Defined in: [src/types/messages.ts:549](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/messages.ts#L549)

Optional TTL for the cache entry. When omitted, the provider’s default TTL is used.

The accepted value space is provider-specific. For example, the Bedrock provider only accepts the values defined by `BedrockCacheTTL` (`'5m'` and `'1h'`). Other providers may accept different values or ignore this field.