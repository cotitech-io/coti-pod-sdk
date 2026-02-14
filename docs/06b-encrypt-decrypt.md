# Encrypt / decrypt

This page documents `CotiPodCrypto` from `/src/coti-pod-crypto.ts`.

## API

```typescript
CotiPodCrypto.encrypt(value, networkOrUrl, dataType?)
CotiPodCrypto.decrypt(ciphertext, aesKey, dataType?)
```

## Supported `DataType`

- `Bool`
- `Uint8`
- `Uint16`
- `Uint32`
- `Uint64`
- `Uint128`
- `Uint256`
- `String`

## Network resolution

Built-in aliases:

- `testnet` -> `https://pod-encryption-service-testnet.coti.io`
- `mainnet` -> `https://pod-encryption-service-mainnet.coti.io`

You can also pass a full custom base URL.

## Return shapes from `encrypt`

- Scalar types: `{ ciphertext: string, signature: string }`
- String type: `{ ciphertext: { value: string[] }, signature: string[] }`

## Example: encrypt and submit for `itUint64`

```typescript
import { CotiPodCrypto, DataType } from "@coti/pod-sdk";

const enc = await CotiPodCrypto.encrypt("42", "testnet", DataType.Uint64);
if (Array.isArray((enc as any).signature)) throw new Error("expected scalar signature");

const itAmount = {
  ciphertext: BigInt((enc as { ciphertext: string }).ciphertext),
  signature: (enc as { signature: string }).signature,
};

// await contract.somePrivateMethod(itAmount)
```

If the returned signature is not already a hex bytes string, convert/normalize it to the bytes format your contract call library expects.

## Example: decrypt callback output

```typescript
const amount = CotiPodCrypto.decrypt("0x...", accountAesKey, DataType.Uint64);
const flag = CotiPodCrypto.decrypt("0x...", accountAesKey, DataType.Bool);
const text = CotiPodCrypto.decrypt({ value: ["123", "456"] }, accountAesKey, DataType.String);
```

## Behavior details

- Empty ciphertext (`""`, `"0x"`, `"0x0"`) returns `"0"` for numeric types and `"false"` for bool.
- String decrypt accepts object form (`{ value: ... }`) or JSON string form.

## Error behavior

`encrypt(...)` throws when:

- HTTP response is non-2xx,
- required fields are missing in response payload.

`decrypt(...)` throws when:

- AES key is missing/empty,
- string ciphertext JSON is invalid.

## Correctness rules

- Encrypt with the same logical type expected by Solidity argument.
- Decrypt with the same logical type returned by callback result.
- Keep numeric widths consistent end-to-end.
