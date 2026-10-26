# ADR 0001: Where post-quantum security enters x402

- Status: Proposed
- Date: 2026-10-26
- Deciders: Project lead
- Supersedes: none

## Context

x402 payment authorizations for EVM chains are signed with classical cryptography. We want an agent's payment to be authorized with a post-quantum signature (ML-DSA, FIPS 204) without breaking compatibility with existing x402 infrastructure.

x402's `exact` scheme already supports deployed ERC-4337 smart accounts through EIP-1271 and counterfactual accounts through ERC-6492 (documented, not tested by us with ML-DSA). The signature bytes are opaque to the protocol. This suggests the post-quantum check can live inside the smart account.

Constraints:

- ML-DSA signatures are large (2,420 bytes for ML-DSA-44, 3,309 bytes for ML-DSA-65), which affects HTTP header size.
- On-chain ML-DSA verification is estimated at roughly 72x to 190x the gas cost of ECDSA.
- Token contracts must route smart-account signatures to `isValidSignature`. Older tokens may not.
- Quantum Wallet, the intended account implementation, is not yet verified to exist as described.

## Options considered

### Option A: use the existing `exact` scheme with an ML-DSA smart account

The ML-DSA verification happens in the account's `isValidSignature`. x402 messages are unchanged.

- Pros: no protocol change, works with existing facilitators and clients if the token cooperates, smallest surface area.
- Cons: depends on token behavior, large signature in the payment header, gas cost borne at settlement.

### Option B: a new scheme or extension (provisionally `exact-pq`)

The payload carries the ML-DSA signature and account details explicitly. A custom facilitator verifier checks the signature.

- Pros: full control over format and verification, independent of token signature routing.
- Cons: custom facilitator and client required, existing hosted facilitators will not understand it, ecosystem fragmentation, more code to maintain and audit.

### Option C: ERC-4337 UserOperation settlement

The payment is a UserOperation executed through a bundler, as in a third-party x402 extension draft.

- Pros: natural fit for smart accounts, programmable settlement.
- Cons: heavier flow, requires a bundler, further from standard x402.

## Decision

Design and validate **Option A first**. Adopt **Option B as the documented fallback**. Option C is recorded as a variant of B and is not pursued unless B is chosen and a bundler-based flow offers clear benefits.

The architecture keeps a single seam, the scheme module, so that switching from A to B changes only that module and the facilitator verifier plugin. The policy engine, wallet adapter, and settlement tracker are unchanged.

## Gates and switch rule

Option A is confirmed only if gates A1 to A5 in `CAPABILITIES.md` reach Verified:

| Gate | Condition for A |
|---|---|
| A1 | A target token accepts the signature form and routes to `isValidSignature` |
| A2 | A facilitator verifies and settles the payment without limit errors |
| A3 | The payment header fits within common header limits |
| A4 | Measured verification gas is acceptable for the intended use cases |
| A5 | A working ML-DSA account and verifier exist on a testnet |

Switch to Option B if any of A1, A2, or A3 fails and cannot be fixed by configuration, or if A4 shows cost is unacceptable for every target use case. If A5 fails because Quantum Wallet cannot be verified, first try a substitute ML-DSA account. A5 failing for all candidates is a project-level risk, not an A-versus-B decision.

## Consequences

- We commit early to spikes that test token behavior and facilitator limits before writing production code.
- If A succeeds, we ship with no changes to x402 and can contribute a tested compatibility matrix.
- If A fails, the failing gate becomes a documented finding that justifies B.
- Under both options the security claim stays narrow: only the payment authorization is post-quantum. Transaction submission, consensus, and receipts remain classical (see the threat model).
- We must track the existing IETF drafts on post-quantum x402 credentials and receipts to avoid duplicating or conflicting with them.

## Revisit when

- A gate result changes the evidence.
- x402 adds a native post-quantum scheme.
- A mainstream token adds explicit support for non-ECDSA smart-account signatures.
- Ethereum adds a native or precompiled ML-DSA verification path that changes the gas calculation.
