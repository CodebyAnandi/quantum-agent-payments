# Security policy

This is a research and design project. It is not audited and must not be used with real funds.

## Reporting a vulnerability

Do not open a public issue for security problems. Use GitHub's private vulnerability reporting on this repository (Security tab, "Report a vulnerability"), or contact the maintainer privately.

Please include what you found, how to reproduce it, and the impact you expect. We aim to acknowledge reports within 7 days.

## Rules for contributors

- Never commit private keys, mnemonics, or `.env` files, even testnet ones.
- Use throwaway keys generated for testing only.
- Do not describe any part of this project as "quantum-safe" without the caveats in `docs/04-security/threat-model.md`.
- Mark unverified claims as Unverified in `CAPABILITIES.md`.
