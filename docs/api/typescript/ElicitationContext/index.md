```ts
type ElicitationContext = RequestHandlerExtra<ClientRequest, ClientNotification>;
```

Defined in: [src/types/elicitation.ts:12](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/types/elicitation.ts#L12)

Context provided to an elicitation callback, including the abort signal for the in-flight request.