# EPF Cohort 6 — zkVM Research and Grandine zkVMs (Pico, Zisk) Integration

This repository is a **learning and progress journal** from [Ethereum Protocol Fellowship (EPF) Cohort 6](https://github.com/eth-protocol-fellows/cohort-six). The technical focus is **proving Ethereum’s consensus-layer beacon state transition (STF)** inside **zkVMs**, using [Grandine](https://github.com/grandinetech/grandine)’s modular `zkvm` host/guest framework. My contribution in that cohort project was integrating **[Brevis Pico](https://github.com/brevis-network/pico)** alongside existing stacks such as RISC Zero and SP1.

If you build or research **clients, consensus specs, or proving systems**, this repo ties together **how a real client wires SSZ inputs into a guest program**, which **crypto paths dominate** (SHA-256, BLS12-381), and how teams **CI multiple zkVM backends**—with pointers to the actual Rust changes upstream.

## What is here vs where the code lives

**In this repo (notes and narrative, not actual source code):**

| Area | Purpose |
|------|---------|
| [study/](study/) | Background: Ethereum protocol context, zkVM landscape (incl. Boundless / ethproofs pointers), Lighthouse/local devnet notes, Cyfrin Solidity & Foundry notes, and misc. client/ERC/DeFi notes. Start with [study/zkvm-boundless.md](study/zkvm-boundless.md) and [study/ethereum.md](study/ethereum.md). |
| [weekly-update/](weekly-update/) | Bi-weekly logs (weeks 4–20) plus the capstone write-up [weekly-update/final-report.md](weekly-update/final-report.md). |
| [eth-meetings/](eth-meetings/) | Short notes from protocol PM / community calls (e.g. ACD-style threads, eth-proof). |

**Upstream implementation (clone these to build or benchmark):**

- Grandine zkVM framework: [github.com/grandinetech/grandine/tree/develop/zkvm](https://github.com/grandinetech/grandine/tree/develop/zkvm)
- Brevis Pico integration and host refactor: [grandinetech/grandine#386](https://github.com/grandinetech/grandine/pull/386)
- Zisk integration and host refactor: [grandinetech/grandine#475](https://github.com/grandinetech/grandine/pull/475)
- Shared precompiles tuned for zk targets (SHA-256, BLS12-381): grandinetech/universal-precompiles [PR #3](https://github.com/grandinetech/universal-precompiles/pull/3), [PR #4](https://github.com/grandinetech/universal-precompiles/pull/4), [PR #5](https://github.com/grandinetech/universal-precompiles/pull/5), [PR #6](https://github.com/grandinetech/universal-precompiles/pull/6).

**Cohort project description:**

[zkVMs for Beacon Chain Snarkification](https://github.com/eth-protocol-fellows/cohort-six/blob/master/projects/grandine_beacon_zkVMs_snarkification.md) (see also [project ideas](https://github.com/eth-protocol-fellows/cohort-six/blob/master/projects/project-ideas.md)).

## Suggested reading order

1. **[weekly-update/final-report.md](weekly-update/final-report.md)** — architecture (host vs guest, execute vs prove), PR summary, and informal benchmark takeaways.
2. **[weekly-update/README.md](weekly-update/README.md)** — week-by-week breadcrumbs if you want the timeline.
3. **[study/README.md](study/README.md)** — curated study index for deeper context.

## Artifacts

- **Fellowship hub:** [github.com/eth-protocol-fellows/cohort-six](https://github.com/eth-protocol-fellows/cohort-six)
- **Conclusion presentation:** [Google Slides](https://docs.google.com/presentation/d/1a1n-GMzIe9ALcUcrq4O_Tme6zmKSvh-lfnIxbqo7l5o/edit?usp=sharing)

## License

Study notes and markdown in this repository are provided under the [MIT License](LICENSE) unless stated otherwise in a specific file.
