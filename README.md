# @coti/pod-sdk

Coti PoD SDK — Solidity contracts and TypeScript utilities for PoD dApps.

## Installation

```bash
npm install @coti/pod-sdk
```

## Usage

### Solidity

```solidity
import "@coti/pod-sdk/contracts/IInbox.sol";
```

### TypeScript

```typescript
import { CotiPodCrypto, DataType, type EncryptedValue } from "@coti/pod-sdk";

// Encrypt (default: Uint64). Use DataType for other types: Bool, Uint8, Uint16, Uint32, Uint64, Uint128, Uint256, String
const encrypted = await CotiPodCrypto.encrypt("42", "testnet", DataType.Uint64);

// Decrypt with the user's AES key (same DataType as used for encrypt)
const decrypted = CotiPodCrypto.decrypt(ciphertextHex, userAesKey, DataType.Uint64);
```

## Foundry

Foundry will automatically create remappings for the package. Ensure `@coti/pod-sdk` is in your `package.json` dependencies.

## Releases

**Current version:** 0.1.0

### Publishing (maintainers)

1. Update version: `npm version patch|minor|major`
2. Build and publish: `npm publish --access public`

### Version history

| Version | Date | Notes |
|---------|------|-------|
| 0.1.0   | —    | Initial release: Solidity contracts (IInbox, MpcLib, MpcAbiCodec) and CotiPodCrypto TypeScript utilities |
