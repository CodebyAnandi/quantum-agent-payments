# Contributing

## Workflow

1. Open or pick an issue.
2. Branch from `main`. Keep changes small.
3. Open a pull request. `main` is protected and requires a PR, even for the maintainer.
4. Docs CI must pass (Markdown lint and link check).

## Rules

- **Evidence first.** Any new capability claim goes into `CAPABILITIES.md` with a status label (Verified, Documented, Estimated, Unverified, Roadmap, Broken) and a source.
- **Decisions in ADRs.** Significant choices get a numbered file in `docs/adr/`.
- **Spikes before code.** Validate assumptions in `spikes/` and record results in `docs/research/` before adding anything to `src/`.
- **Diagrams as Mermaid** in Markdown so they diff cleanly.
- **No real funds, no committed secrets.**

## Commit messages

Short imperative summary, for example: `Add A1 spike plan`. Reference the issue number when there is one.
