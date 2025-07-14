## Interested tasks

k1> Grandine: zkVMs for Beacon Chain Snarkification
  https://github.com/eth-protocol-fellows/cohort-six/blob/master/projects/project-ideas.md#grandine-zkvms-for-beacon-chain-snarkification

  By Saulius Grigaitis

  We are experimenting with zkVMs (zero-knowledge virtual machines) to enable SNARK-based proving of the Beacon Chain’s state transition function. Initial results using SP1 and RISC Zero show promise for networks with tens of thousands of validators. We now propose to scale this work to support larger validator sets and to explore new zkVMs — such as OpenVM and Zisk — that offer continuation supports.

k2> Ream Client - A Beam client in Rust: Benchmark zkVM performance on Ream's Beacon state transition functions
  https://github.com/eth-protocol-fellows/cohort-six/blob/master/projects/project-ideas.md#ream-client---a-beam-client-in-rust-benchmark-zkvm-performance-on-reams-beacon-state-transition-functions

  By Unnawut

  We've started benchmarking SP1 and RISC Zero against ream's consensus codebase, giving us early data on performance and how different zkVMs behave when running consensus logic. This lets us track performance regressions as we iterate on the codebase and pinpoint the specific roadblocks and bottlenecks that arise when running consensus specs inside zkVM environments.

  We are looking to extend our benchmarking setup to cover other zkVMs like OpenVM, Zisk, Jolt, Brevis Pico, Valida, etc. You'll be implementing benchmarks for these systems, modifying the ream codebase as needed to support the snarkification, while keeping the benchmark framework modular enough to keep track of differences across code iterations, and to easily convert for beam chain specs down the road.

  We hope this project provides essential data for making informed zkVM choices in Beam Chain development. For the fellow, it's a chance to get hands-on with both beacon chain consensus implementation details and the rapidly evolving zkVM ecosystem, and prepare yourself to snarkify the upcoming Beam Chain specs.

- Lodestar: ERA file support
  https://github.com/ChainSafe/lodestar/issues/7048

- Teku / Nimbus / Grandine: Implement EIP-7917 - Deterministic proposer lookahead
  [EIP-7917](https://eips.ethereum.org/EIPS/eip-7917) is a mechanism to pre-calculate and store a deterministic proposer lookahead in the beacon state at the start of every epoch. Implementing this EIP in one or more of the above mentioned clients is a low hanging-fruit as well as [super valuable](https://hackmd.io/@linoscope/eip-7917-from-preconf-protocol), and may serve as an excellent starting point for a fellow to get involved with core protocol work.

  By Justin Drake & Lin Oshitani

- Grandine: Exploring Distributed Validator Technology (DVT) Compatibility
  By Saulius Grigaitis

  Distributed Validator Technology (DVT) is gaining momentum within the Ethereum staking ecosystem. We propose to assess Grandine’s compatibility with major DVT solutions, evaluate integration challenges, and explore potential extensions to support secure, decentralized validator operations.

- Grandine: ePBS (EIP-7732)
  By Saulius Grigaitis

  Grandine currently has no implementation of EIP-7732: Enshrined Proposer-Builder Separation (ePBS).

- Ephemery testnet
  By Mario Havel

## Info
- start at June 16, 2025, count as wk 1
- currently (Jul 14) in epf-6: week 4 / week 5
- standup call: every Monday @2300 HKT, https://meet.ethereum.org/epf-standup
- OH call: every Wed @2300 HKT, https://meet.ethereum.org/epf-office-hours

### Week 3 - Cannes week

- https://efdn.notion.site/EPF6-EthCC-in-Cannes-208d9895554180c09da2e5c1f70e1b09#208d98955541819e8eded62554f61628

### Consensus Client

Client Diversity: https://clientdiversity.org/#distribution


Grandine, Rust, https://github.com/grandinetech/grandine
Lighthouse, Rust, https://github.com/sigp/lighthouse/
Loadstar, Typescript, https://github.com/ChainSafe/lodestar
Ream, Rust

### Execution Client
Erigon, golang
Geth, golang
Nethermind, C# / .NET
Reth, Rust, https://github.com/paradigmxyz/reth
