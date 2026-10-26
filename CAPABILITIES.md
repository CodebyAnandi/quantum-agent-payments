# Capabilities

Every claim in this project carries an evidence label. Nothing marked Unverified or Roadmap may appear in a diagram or document as fact.

Last reviewed: 2026-10-26

## Status legend

| Label | Meaning |
|---|---|
| Verified | We tested it ourselves. Link the spike, test, or transaction. |
| Documented | Stated in a primary source. Not tested by us. |
| Estimated | A published or computed figure. Not measured by us. |
| Unverified | Needs a spike before we rely on it. |
| Roadmap | Announced by the project as planned. Not usable today. |
| Broken | Tested and does not work. Record why. |

## Viability gates for Option A

| ID | Capability | Status | Evidence | Next action |
|---|---|---|---|---|
| A1 | Token accepts the bytes-signature form of `transferWithAuthorization` and routes smart-account addresses to `isValidSignature` | Unverified | x402 wallet compatibility docs warn that older ERC-3009 tokens use `ecrecover` only and treat every address as an EOA | Spike `a1-token-signature-routing` on a local chain |
| A2 | Facilitator pre-verification accepts a non-ECDSA ERC-1271 signature of ML-DSA size without hitting gas or length limits | Unverified | None yet | Run the open-source facilitator locally, then a hosted one on a testnet |
| A3 | Payment header with an ML-DSA signature fits within client, proxy, and server header limits | Unverified | FIPS 204 sizes: ML-DSA-44 signature 2,420 bytes, ML-DSA-65 signature 3,309 bytes. Base64 adds about one third | Send real payloads through common stacks |
| A4 | On-chain ML-DSA verification gas is acceptable for target payment sizes | Estimated | PQSigAbstract paper estimates roughly 72x to 190x ECDSA cost (amortized) | Measure on a local chain |
| A5 | A working ML-DSA verifier and account exist on a public testnet | Unverified | PQSigAbstract is a design paper. No deployed implementation confirmed | Verify Quantum Wallet or choose a substitute |
| A6 | Counterfactual (ERC-6492) accounts work with ML-DSA signatures | Unverified | x402 docs list ERC-6492 for `exact` with configuration, not tested with ML-DSA | Optional spike |

## x402

| Capability | Status | Evidence |
|---|---|---|
| `exact` scheme supports deployed ERC-4337 smart accounts via EIP-1271 | Documented | x402 wallet compatibility page |
| `exact` scheme supports ERC-6492 counterfactual accounts | Documented | x402 wallet compatibility page, "supported with configuration" |
| Smart contract wallets register for `exact` only in Coinbase's CDP SDK | Documented | CDP x402 docs |
| Permit2 as an alternative transfer method (v2) | Documented | x402 EVM crate documentation |
| Native post-quantum scheme | Not available | No mention found in x402 documentation |
| Smart-account extension using ERC-4337 UserOperations | Roadmap | Third-party draft by Nevermined, not part of core x402 |

## ERC-8004 (optional module)

| Capability | Status | Evidence |
|---|---|---|
| Identity, Reputation, and Validation registries | Documented | EIP page |
| Standard status | Documented | EIP page lists it as Draft |
| Payment proofs referenced in off-chain feedback files | Documented | EIP text and Polygon documentation |
| Testnet deployments | Unverified | Polygon docs mention Amoy. Confirm addresses before use |

## Quantum Wallet (`quantum.phd`)

| Capability | Status | Evidence |
|---|---|---|
| Product exists with public documentation and contracts | Unverified | Not found in our searches. Claim came from a pasted summary |
| ML-DSA signatures | Unverified | Same |
| ERC-4337 smart accounts | Unverified | Same |
| Arbitrum and Quantum L1 support | Unverified | Same |

If these cannot be verified, substitute another ML-DSA smart-account implementation and record the change in an ADR.

## Related work

| Item | Status | Evidence |
|---|---|---|
| IETF draft: post-quantum credential binding for x402 (Hopley) | Documented | draft-hopley-x402-pqc-credential-binding-00, June 2026. Covers agent credentials, not payment signing |
| IETF draft: post-quantum discipline for x402 STARK receipts (Vauban) | Documented | draft-vauban-x402-pqc-receipts-01, May 2026. Covers receipts |
| PQSigAbstract: modular post-quantum verification for Ethereum | Documented | University of Colorado Boulder, June 2026 |

## Sources

- x402 wallet compatibility: https://docs.x402.org/advanced-concepts/wallet-compatibility.md
- CDP x402 SDK: https://docs.cdp.coinbase.com/x402/core-concepts/cdp-sdk
- ERC-8004: https://eips.ethereum.org/EIPS/eip-8004
- Nevermined smart-account extension: https://nevermined.ai/docs/specs/x402-smart-accounts.md
- Hopley draft: https://datatracker.ietf.org/doc/draft-hopley-x402-pqc-credential-binding/
- PQSigAbstract: https://scholar.colorado.edu/concern/reports/05741t70f
