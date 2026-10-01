# Contributing

Thanks for helping build the list of AI/LLM inference tooling for AMD RDNA GPUs!

## Scope

- Must be about running AI/ML workloads on RDNA GPUs: RDNA 3 (`gfx1100`), RDNA 3.5 (`gfx1150`/`gfx1151`, Strix Point / Strix Halo), RDNA 4 (`gfx1200`/`gfx1201`, RX 9000 / Radeon AI PRO R9700).
- Datacenter CDNA (MI-series) content is out of scope.
- The project must be maintained, documented, and actually work on target hardware. A README that shows measured numbers beats one that promises them.

## How to add an entry

1. Pick the right section (`llama.cpp`, `vLLM`, `Containers and tooling`, `Community`), or propose a new one if nothing fits.
2. Add it in alphabetical order within the section, using this exact format:

   ```markdown
   - [Name](repo-or-site-url) — Description ending in a period.
   ```

3. The description should say what it is **and** which hardware it targets. One to two lines, no marketing superlatives ("fastest ever") — measured claims are fine if the linked repo backs them.
4. Open a pull request against `main` with your addition.

## Guidelines

- **No duplicates.** If a repo is a fork, link the fork only when it carries meaningful extra work — and say whose work it builds on.
- **Dead links get removed.** Entries are periodically checked; if the upstream project is gone or unmaintained, the entry goes.
- **No self-promotion dumps.** One entry per project is fine, even your own — just keep the description factual.
- Don't add badges, stars, or install counts to entries. The list is for discovery, not ranking.

For bug reports, section proposals, or questions about scope, open an issue.
