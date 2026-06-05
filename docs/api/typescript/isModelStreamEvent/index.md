```ts
function isModelStreamEvent(event): event is ModelStreamEvent;
```

Defined in: [src/models/streaming.ts:44](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/models/streaming.ts#L44)

Type guard to check if an event with a type discriminator is a ModelStreamEvent.

## Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `event` | { `type`: `string`; } | The event to check |
| `event.type` | `string` | \- |

## Returns

`event is ModelStreamEvent`

true if the event is a ModelStreamEvent