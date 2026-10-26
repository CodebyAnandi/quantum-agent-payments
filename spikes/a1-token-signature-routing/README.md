# Spike A1: token signature routing

## Question

Does a token implementing EIP-3009 accept a smart-account signature in the bytes form of `transferWithAuthorization` and route it to the account's `isValidSignature`?

## Why it matters

Option A depends on this. If the token only uses `ecrecover`, the ML-DSA signature never reaches the account and Option A fails.

## Hypothesis

A token that code-routes (uses a SignatureChecker-style check) will accept a signature from a smart account that implements ERC-1271. A mock account that accepts any signature starting with a known marker is enough to prove routing before involving real ML-DSA.

## Method

1. Start a local chain (Foundry Anvil).
2. Deploy a mock EIP-3009 token with a code-routing signature check.
3. Deploy a mock ERC-1271 account that returns the magic value for a test signature.
4. Call `transferWithAuthorization` (bytes variant) with the account as payer.
5. Repeat with an ecrecover-only token and record the failure.
6. Repeat with a signature of ML-DSA-44 size (2,420 bytes) and ML-DSA-65 size (3,309 bytes) to check for size limits.
7. If possible, fork a public testnet and repeat against the real test token used by the x402 reference setup.

## Success criteria

- Code-routing token: transfer succeeds, account's `isValidSignature` is called.
- Large signature: still succeeds, gas recorded.
- Ecrecover-only token: fails as expected.

## Failure criteria

- Target token fails with a valid account signature, or rejects large signatures.

## Result

Not run yet. Record: date, token and version, chain, commands, gas used, pass or fail, and link to the commit. Then update `CAPABILITIES.md` row A1.
