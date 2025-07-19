# Ethereum Protocol Team

new structure
  https://blog.ethereum.org/2025/06/02/announcing-protocol

protocol:
key priority: scale L1, scale blobs, improve UX

ppl:
Scale L1: Tim Beiko & Ansgar Dietrichs
Scale L2: Alex Stokes & Francesco D’Amato
Improve UX: Barnabé Monnot & Josh Rudolf

website: https://protocol.ethereum.foundation/
team: https://protocol.ethereum.foundation/teams

Meeting and Project Management:
https://github.com/ethereum/pm/issues

## Protocol Research Call (PRC) 2
- DA is important. It captures Defi fee

# Ethereum - Beam Day Cannes

## Beam Day Event Overview
https://hackmd.io/@willcorcoran/BkZ-gs5Bll

Beam Day playlist
https://www.youtube.com/playlist?list=PLJqWcTqh_zKEwDCbJ31ds3VSIxa4rUxvF

## Opening
Justin Drake (EF)
https://hackmd.io/@willcorcoran/S1IeXoqSgg

the short term goal (2025 mid - 2026 mid)
  - scaling L1
  - scaling the blob (scaling L2)
  - improving UX

In the long-term
  - make ethereum post-quantum

## Post-quantum signautre for beam chain

https://hackmd.io/@willcorcoran/SJAxOscree
Dmitry Khovratovich (EF researcher)

- Basically it will be poseidon hash based signature.
- signature based on chaining and picking from the position that sum up to be a certain speciific number.

## SNARKIFICATION: Beacon SNARKification (Boundless)
https://www.youtube.com/watch?v=_9JM83Comv4
Jacob Everly (Boundless)

- CCTP stands for Circle's Cross-Chain Transfer Protocol. It is expensive. LayerZero is charging 500k for integration to bring liquidity on chain.
- boundless is on interop - ZKasper
- boundless market make it cheap to generate zkproof

- Zkasper is thing that boundless is working on
- proving for a block
- RiscZero zkVM is reaching v3

## SNARKIFICATION: Zig Client and zkVM Integration (Zeam & EF)

https://hackmd.io/@willcorcoran/rkHcOo9Hex

By Guillaume Ballet (GB) Zeam & EF - Geth

- Geth to support multiple zkVM prover.
- riskZero zkVM seems to be doing it properly, at least is getting the endorsement from Geth developer

# EPF wiki study notes

## Consensus layer - Core dev

about protocol guild
  - https://tim.mirror.xyz/srVdVopOFhD_ZoRDR50x8n5wmW3aRJIrNEAkpyQ4_ng
  - https://www.youtube.com/watch?v=9Tc2g7pu-gc

## Testing

They are important and ensure different clients adhere to the same standard
- Ethereum test: https://github.com/ethereum/tests (for previous version before the merge)
- EL spec: https://github.com/ethereum/execution-specs
  - EL test docs: https://eest.ethereum.org/main/running_tests/
- Solidity test: https://github.com/ethereum/solidity
- CL spec: https://github.com/ethereum/consensus-specs

Cross-layer testing - e2e testing - execution & consensus layer
  - hive: https://github.com/ethereum/hive
    - simulator, setting a local test-net (2 nodes, CL+EL combined), and run test
  - assertoor (ethpandaops)
  - kurtosis (ethereum-package): spawning ethereum devnet

eth1 - legacy name of execution layer
eth2 - legacy name of consensus layer

# Ethereum Execution Spec Tests
- execution specs: https://github.com/ethereum/execution-specs
- execution spec tests: https://github.com/ethereum/execution-spec-tests

## Filling the tests
https://eest.ethereum.org/main/filling_tests/

Steps
1. test cases (in python) -> test fixtures (aka filling the tests)
2. Client (or test tools i.e. Hive) consumes JSON fixtures,

There is also "execute" command directly take the test cases to the test client.

The filling command, example

`uv run fill tests/prague -k "bls12_g1add" -v -m state_test --clean`

- `uv` - the python package manager, written in rust
- `run` - the command
- `-k` - filter by test case ID
- `-v` - verbose
- `-m` - type of test to run
- `--clean` - clean up the fixture dir

## Consume Direct
https://eest.ethereum.org/main/running_tests/consume/direct/

- only support el-geth and el-nethermind

examaple:

```sh
# for geth
uv run consume direct --input ./fixtures -m state_test --bin=evm

# for nethermind
uv run consume direct --input ./fixtures -m state_test --bin=nethtest
```

## Consume Engine & RLP

- engine API: from consensus layer
- RLP: run during sync'ing
- Run both engine and RLP to add redundency


consuming fixture using engine API. Comm between the consensus client.

# Ethereum Consensus Spec Tests
- consensus specs: https://github.com/ethereum/consensus-specs
- consensus spec tests: https://github.com/ethereum/consensus-spec-tests

... not sure how to run the spec and spec test yet.

# Hive

# Kurtosis

- kurtosis is like a container orchestrator with config and instance for spawning containers.

- you will then need the etheureum-package
  https://github.com/ethpandaops/ethereum-package

- ethpandas blog post:
  https://ethpandaops.io/posts/kurtosis-deep-dive

- full configuration list:
  https://github.com/ethpandaops/ethereum-package?tab=readme-ov-file#configuration


# EF Protocol Jul 4 - 11 Update
progress update:
  https://x.com/ladislaus0x/status/1943612099600466288

Increase max transaction gas limit to 2^24 per tx, about 16.77M gas

L1 zkEVM realtime proving:
  https://blog.ethereum.org/2025/07/10/realtime-proving

There is a ePBS and FOCIL compatibility discussion:
  https://ethereum-magicians.org/t/epbs-focil-compatibility/24777
  ePBS [EIP-7732](https://eips.ethereum.org/EIPS/eip-7732): Enshrined Proposer-Builder Separation
  FOCIL [EIP-7805](https://eips.ethereum.org/EIPS/eip-7805): Fork-choice enforced inclusion list

A nice UI: https://cperezz.github.io/bloatnet-website/index.html

# Others

- Reconfiguring on AllCoreDevs (ACD) organization and how Ethereum governance works
  https://ethereum-magicians.org/t/reconfiguring-allcoredevs/23370

- Ethereum Protocol Fellowship: https://epf.wiki/#/README

# Todo

How to participate in ethereum protocol dev?
  - apply job:
    - https://jobs.lever.co/ethereumfoundation/07366f81-c0a4-4ddb-ad66-b6c95fc217dc
    - https://jobs.lever.co/ethereumfoundation/824e5aa6-cda4-438b-8901-125ab53cdacb

  - running Eth validator (via rocketpool)
  - running boundless validator

  - followup ethereum protocol fellowship: Cohort 6
  - study deeper in riscZero zkVM
  - running boundless zkprover

# Info

Client Diversity: https://clientdiversity.org/#distribution

## Consensus Client
Grandine, Rust, https://github.com/grandinetech/grandine
Lighthouse, Rust, https://github.com/sigp/lighthouse/
Loadstar, Typescript, https://github.com/ChainSafe/lodestar
Prysm, golang, offchainlabs (company behind Arbitrum)
Ream, Rust

## Execution Client
Besu, Java, Hyperledger
Erigon, golang, Ledgerwatch
Geth, golang, EF
Nethermind, C# / .NET, Nethermind
Reth, Rust, https://github.com/paradigmxyz/reth, Paradigm
Ethereumjs, typescript, EF
