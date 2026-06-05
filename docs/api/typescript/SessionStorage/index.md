```ts
type SessionStorage = {
  snapshot: SnapshotStorage;
};
```

Defined in: [src/session/storage.ts:26](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/session/storage.ts#L26)

SessionStorage configuration for pluggable storage backends. Allows users to configure snapshot and transcript storage independently.

## Example

```typescript
const storage: SessionStorage = {
  snapshot: new S3Storage({ bucket: 'my-bucket' })
}
```

## Properties

### snapshot

```ts
snapshot: SnapshotStorage;
```

Defined in: [src/session/storage.ts:27](https://github.com/strands-agents/sdk-typescript/blob/00e04880c30c5ce1f76e40c164d32ce52f7b6dca/strands-ts/src/session/storage.ts#L27)