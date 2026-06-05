Defined in: [src/hooks/events.ts:81](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/hooks/events.ts#L81)

Base class for all events yielded by `agent.stream()`. Carries no hookability — subclasses that should be hookable extend [HookableEvent](/docs/api/typescript/HookableEvent/index.md) instead.

## Extended by

-   [`HookableEvent`](/docs/api/typescript/HookableEvent/index.md)

## Constructors

### Constructor

```ts
new StreamEvent(): StreamEvent;
```

#### Returns

`StreamEvent`