Defined in: [src/retry/backoff-strategy.ts:28](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/retry/backoff-strategy.ts#L28)

Computes the delay before the next retry attempt.

## Methods

### nextDelay()

```ts
nextDelay(ctx): number;
```

Defined in: [src/retry/backoff-strategy.ts:35](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/retry/backoff-strategy.ts#L35)

Returns the delay in milliseconds before the next attempt.

Must be a non-negative finite number. Implementations should treat `ctx.attempt < 1` as a programmer error.

#### Parameters

| Parameter | Type |
| --- | --- |
| `ctx` | [`BackoffContext`](/docs/api/typescript/BackoffContext/index.md) |

#### Returns

`number`