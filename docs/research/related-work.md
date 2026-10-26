# Related work

Last reviewed: 2026-10-26. This is not an exhaustive survey. Verify before claiming a gap.

| Work | What it covers | How this project differs |
|---|---|---|
| IETF draft `draft-hopley-x402-pqc-credential-binding-00` (June 2026) | Binds Falcon-1024 and ML-DSA-65 credentials to x402 agent payment authorization, with gateway verification and session tokens | Credentials and sessions, not the payment signature itself |
| IETF draft `draft-vauban-x402-pqc-receipts-01` (May 2026) | Hybrid ES256K plus ML-DSA-65 dual-signed receipts and STARK-based proofs for audit retention | Receipts and records, not payment authorization |
| PQSigAbstract (University of Colorado Boulder, June 2026) | Modular post-quantum signature verification architecture for Ethereum, ERC-4337 compatible, with gas models for ML-DSA, Falcon, SLH-DSA | Design paper for on-chain verification. We test it inside an x402 payment path |
| x402 wallet compatibility documentation | Documents `exact` support for ERC-4337 accounts via EIP-1271 and ERC-6492 | Our Option A relies on this and tests it with ML-DSA |
| Nevermined x402 smart-account extension | Proposes ERC-4337 UserOperation settlement for x402 | A variant of our Option B |
| ERC-8004 (Draft) | Agent identity, reputation, and validation registries | Optional module. Payments are out of its scope |

## Open follow-ups

- Read both IETF drafts in full and decide whether to align formats.
- Look for working ML-DSA smart-account deployments (including Quantum Wallet).
- Check whether any hosted facilitator advertises non-ECDSA smart-account support.
