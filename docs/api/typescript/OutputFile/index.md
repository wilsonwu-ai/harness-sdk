Defined in: [src/sandbox/types.ts:46](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/sandbox/types.ts#L46)

A file produced as output by code execution.

Used to carry binary artifacts (images, charts, PDFs, compiled files) from sandbox execution back to the agent. Shell-based sandboxes typically return an empty array. Jupyter-backed or API-backed sandboxes can populate this with generated artifacts.

## Properties

### name

```ts
readonly name: string;
```

Defined in: [src/sandbox/types.ts:47](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/sandbox/types.ts#L47)

---

### content

```ts
readonly content: Uint8Array;
```

Defined in: [src/sandbox/types.ts:48](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/sandbox/types.ts#L48)

---

### mimeType

```ts
readonly mimeType: string;
```

Defined in: [src/sandbox/types.ts:49](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/sandbox/types.ts#L49)