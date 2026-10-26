# Threat model

Status: Draft v0.1. Applies to the Option A design; revisit if Option B is chosen.

## Assets

- The agent account's funds
- The ML-DSA private key
- Payment authorizations in flight
- Payment records and receipts kept for audit

## What is and is not post-quantum

| Link | Primitive | Post-quantum? |
|---|---|---|
| Agent payment authorization | ML-DSA via smart account | Yes, if the full verification path is correct |
| Facilitator transaction submission | ECDSA | No |
| Chain consensus and block signatures | Chain-specific, classical | No |
| ERC-8004 identity ownership (if used) | ECDSA-owned token | No |
| x402 payment response and receipts | Classical by default | No |
| TLS between agent and server | Deployment-dependent | Possibly, with hybrid key exchange |
| Token contract logic | Not signature-based | Not applicable |

This project never claims the whole system is quantum-safe. Known gaps are listed here and in `limitations` notes as they are found.

## Threats and mitigations

| Threat | Mitigation |
|---|---|
| Replay of a payment authorization | Unique nonce, `validBefore` expiry, chain id and token in the signed domain, facilitator nonce tracking |
| Downgrade to a classical scheme | Policy engine scheme allowlist. The agent refuses options outside it |
| Amount or payee tampering | Both are in the signed digest. The facilitator cannot change them |
| Gas griefing through expensive verification | Size checks, simulation with a gas cap, per-account rate limits, reject before submitting |
| Key compromise | Spending caps, short-lived authorizations, key rotation in the account, revocation path |
| Malicious facilitator | Cannot forge payments. Can censor or delay. Client may use several facilitators |
| Signature malleability and format confusion | Versioned envelope, strict parsing, canonical encoding checks |
| Bugs in a new verifier | NIST test vectors, differential testing against a reference library, external review before any mainnet use |
| Fake or self-dealing feedback (ERC-8004) | Out of the core flow. Handled in the optional module design |

## Trust assumptions

- The ML-DSA implementation in the wallet and the verifier is correct. This is unproven until tested and reviewed.
- The token contract correctly routes signature checks to the account.
- The chain's finality and consensus are classical and out of scope.

## Out of scope

Production key custody, mainnet deployment, legal and compliance analysis.
