# Report on Brevis Pico zkVM integration & benchmarking of block state transition function in zkVM for Grandine
<!--
This final update is meant to be understandable by anyone who wants to learn about have you done in the cohort without diving into your project proposal and every update. Use it as a summary about current state of things with all relevant information and links.
-->

## Project Abstract

Grandine is an Ethereum client in both consensus and execution layers written in Rust. As Ethereum has an initiative of using zkVMs to generate proofs instead of re-computing the block state for block state transtion function (stf), Grandine team have written an extension framework that could easily integrate any kind of zkVM to benchmak its stf performance. The code of the extension framework is located inside Grandine main repository [`zkvm` directory](https://github.com/grandinetech/grandine/tree/develop/zkvm).

With this framework, Grandine team have already integrated RiscZero [**r0vm**](https://github.com/grandinetech/grandine/tree/develop/zkvm/guest/risc0) and Succinct [**SP1**](https://github.com/grandinetech/grandine/tree/develop/zkvm/guest/sp1) zkVMs, and now want to integrate with other zkVMs.

As there were a few fellows working on this initiative, each of us was assigned to integrate one zkVM.

- [me](tk:github): [Brevis Pico](tk)
- [Ritesh](tk:github): [ZKM Ziren](tk)
- [Aman](tk:github): [Zisk](tk)

In this report, I will detail out how I have integrated Brevis Pico zkvm in Grandine, refactored the zkvm extension to be more modular, requested to add one more test case to run, and attempts in running the actual zkvm proof generation in both CPU machine locally and GPU machine via Bonsai Network.

## Detail Work & Final State

### Main zkvm host and guest code

The work described here is covered in [grandinetech/grandine PR #386](https://github.com/grandinetech/grandine/pull/386).

There is a host and guest code section in the zkvm extension architecture. The [host code](https://github.com/grandinetech/grandine/blob/develop/zkvm/host/src/main.rs) section performs the following functionality:

- Based on the test case selected from cli arguments, retrieve the 1) chain/phase config, 2) signed beacon block in [SSZ format](tk), 3) beacon state in SSZ format, and 4) pre-compute and store the public key cache for performance optimization.

- Call the state transition function with the four input paramters above, and store the new state root (the result). At the end we will compare this value with the one computed from zkVM and they should match.

- The host then initiate the zkvm runtime and pass in the four input paramters for computation.

- The host retrieves the final state root computed from the zkvm runtime and compare with its own value.

The [guest code](tk) is run inside the zkvm runtime. There is an implementation of the guest code for each zkVM. It performs the following functionality:

- It retrieves and parses the four input paramters from the host, deserializing the object from SSZ format.

- Run the state transition based on the four inputs, and compute the state root of the new state. It may or may not generate a zk-proof depending on its running mode.

- Return the state root and possibly with the proof back to the host.

There are two running modes for the guest, **execute** mode and **prove** mode. Execute mode will only execute the guest code instructions in zkvm runtime without generating proofs. Prove mode will execute the guest code and generate the zk-proof. Execute mode can be seen as a sanity check that the execution logic and computation is performed correctly, while the zk-proof generation is what we are really interested in and the most time-consuming step.

### Main Grandine PR#386

I first refactored the `zkvm/host/backend.rs` so each zkVM-specific implementation is saved in a separate module (e.g. [risc0](tk) [sp1](tk)). Then I added my implementation of [Pico host code](https://github.com/grandinetech/grandine/pull/386/files#diff-1d8fb10e7eb152891f7c37b4a249dc4092ff33434189694ee7aee7d77cfa5a9e), which is more or less the same as the code of other zkVM by implementing the necessary trait calling the specific zkVM apis.

A [helper build script](https://github.com/grandinetech/grandine/pull/386/files#diff-2015ba7f1b0aad9904659f52f2844d91f022c63a2b10db135b2332668943a4a0) is written so that building the zkvm-host code will automatically trigger building the zkvm guest code as well. Otherwise, two separate commands are needed to compile both the host and guest code. Care need to be taken as Pico requires building with nightly rust version, while the main Grandine repo run with stable rust version. So all pico related build and execution command must be run with prefixing the nighly version, e.g. `cargo +nightly-2025-08-04 ...`.

The [zkVM guest code](https://github.com/grandinetech/grandine/pull/386/files#diff-b622fd118817f3bd6bf7b754cca00afa983d87cc40544a6d0cfd144fe7548529) for Pico is also similar to the guest code for other zkVMs, calling Pico-specific zkvm guest API.

Lastly a github CI workflow script [`zkvm-test.yml`](https://github.com/grandinetech/grandine/pull/386/files#diff-3100b4a9081a2d21a9a64759e0d35e33684fac9cb222c56d8c00135f23beaed6) is added so different zkVM host and guest code get built and tested as new codes/PRs got merged to the codebase. This serves as a sanity check to ensure that the future code change doesn't break the existing zkVM host and guest code.

### Accompanied PRs to SHA2 and BLS12-381

Now the state transition function heavily relies on SHA256 hash function and BLS12-381 elliptic curve based cryptography. Each zkVM has their own tweaks and optimization on running the hash function and elliptic curve arithmetics. So we need to add the own tweaks for Brevis Pico zkVM.

Grandine has a universal-precompile packages of [sha-256](https://github.com/grandinetech/universal-precompiles/tree/RustCrypto-hashes/sha2-v0.10.9) and [bls12-381](https://github.com/grandinetech/universal-precompiles/tree/zkcrypto/bls12_381-6bb9695) that got conditionally turned on for each zkVM based on the compilation target.

To support Brevis Pico, I have to patch these two libraries:

- sha-256: [grandinetech/universal-precompiles PR #3](https://github.com/grandinetech/universal-precompiles/pull/3) and
- bls12-381: [grandinetech/universal-precompiles PR #4](https://github.com/grandinetech/universal-precompiles/pull/4)

The BLS12-381 PR is particularly delicate as there are more than 25 places that need to apply unique customization based on different zkVM optimization. Brevis Pico also have a `target_vendor` string of **risc0**, which is the same as r0vm. So we add an additional feature flag **zkvm-pico** during compilation to distinguish between Pico from r0vm. We also add a **zkvm-risc0**feature flag to make this distinction clear.

This can be seen in:
- bls-crypto library [cargo.toml](https://github.com/grandinetech/grandine/pull/386/files#diff-b9a966a6766d089f8f3954f894313fd1f550d596d3e5d2ecacf8f895fcd89c74)
- sha-256 [cargo.toml](https://github.com/grandinetech/universal-precompiles/pull/3/files#diff-704ad48075ec95eb928ef5912af78756de25009feefecbc6b18e2d0bc5c20a48)
- bls12-381 [cargo.toml](https://github.com/grandinetech/universal-precompiles/pull/4/files#diff-2e9d962a08321605940b5a657135052fbcef87b5e360662bb527c96d9a615542)

### Benchmark Running

There were originally three test cases comprised in the zkVM extension framework. Responding to my request, Grandine team added another empty block transition test coming from the [consensus-spec-test](https://github.com/ethereum/consensus-spec-tests/tree/master/tests/mainnet/electra/sanity/blocks/pyspec_tests/empty_block_transition) to the extension framework as a sanity check.

There are now four test cases, ranked below from the least computing-intensive to the most.

1. An empty block transition test
2. Pectra Devnet without epoch transition
3. Pectra Devnet with epoch transition
4. Mainnet without epoch transition

I ran an informal benchmark on my Macbook Air 2020 with M1 chip and 16GB RAM.

In execute mode, for **r0vm**:

|  | empty-block-transition | pectra-wo-epoch | pectra-w-epoch | mainnet-wo-epoch |
| -------- | -------- | -------- |--|--|
| time elapsed | 5.59s | 117.43s | 927.43s | 3066.35s |
| execution cycles | 241,696,768 | 4,909,432,832 | 39,038,484,480 | 116,489,453,568 |

In execute mode, for **pico**:

|  | empty-block-transition | pectra-wo-epoch | pectra-w-epoch | mainnet-wo-epoch |
| -------- | -------- | -------- |--|--|
| time elapsed | 33.62s | 528.43s | 2592.21s | Panicked with max memory exceeded. [Read explanation](https://github.com/brevis-network/pico/issues/48#issuecomment-3286930946).|
| execution cycles | 164,850,074 | 2,555,459,575 | 12,685,893,340 | n.a. |

Now we are really interested in running the test cases with proof generated. Unfortunately this takes a huge amount of time. Even the simplest test case takes 3 hours onward to run and I had to terminate the test mid-way.

At the end, I was able to get a hand on running proving test on RiscZero [Bonsai Network](tk), with GPU capability, thank to another protocol fellow [Subhasish](tk) who passed his API key to me.

In prove mode, for **r0vm**

|  | consensus-spec-test | pectra-wo-epoch | pectra-w-epoch | mainnet-wo-epoch |
| -------- | -------- | -------- |--|--|
| time elapsed | 58.12s | 469.90s | error out | error out |

Two test cases error out as it seems the computation has exceeded the account computing quota on Bonsai Network.

## Current Impact and Future of the project
<!--
Potential next steps and improvements, use cases in the ecosystem
-->

This is my part of Grandine zkVM integration with Brevis Pico. Other fellows are integrating Ziren (Ritesh) and Zisk (Aman) into Grandine using similar approach.

Though we have achieved significant progress in this project, I also see the work is not fully completed yet. We set out to benchmark running state transition function in various zkVMs. We end up only be able to run `execute` mode in my local laptop for r0vm and pico.

We will need a powerful GPU cluster in order to benchmark the full state transition with proofs generated. Hopefully Grandine team would be able to work the various ecosystem team to get the GPU computing power and benchmark the stf performance in the future.

Another possible direction is that various **proving networks** emerged recently. [Boundless Network](tk) have growh a prover network community, while [SP1 proving network]() have launched three months ago with token launched to incentivized its proving members. Grandine team could also explore sending the state transition function proving requests across to these networks and get the proof generated back.

## Self-evaluation
<!--
- general feedback about your project and activity within EPF
- Be realistic and reflect on your progress, achievements, your satisfaction and criticism
-->

Overall, through the engagement of zkVM integration work via Ethereum Protocol Fellowship program, I become more familiar with how Ethereum clients are tested generally. Imagine there are more than six clients available by different implementation teams, yet they need to work together coherently as a single network. We need a set of thorough test cases that client teams could test against with a unified test APIs. The Ethereum Foundation [consensus-spec](tk) [consensus-spec-test](tk) and [execution-spec-test](tk), and numerous other testing tools (e.g. Kurtosis [Ethereum Package](https://github.com/ethpandaops/ethereum-package)) serve as a good example on how to create a common set of test cases that work across various clients.

I also learned more about Ethereum initiative of integrating zkVM in generating proof rather than re-computing all the state transition function in block production, and come to appreciate the challenge we have to overcome in order to generate real-time proof (currently meaning generating proof in less than 12 sec) in the mainnet environment.

In here, I would like to express my heartfelt gratitude to Ethereum Foundation and Grandine client team and other fellows that help me along in this journey. Ethereum Foundation have allocated resources to support this program so fellows could work and contribute directly with client teams. Grandine team (particularly [Saulius](), [Artiomtr](), and [Povilas]()) have contributed their valuation time in mentoring me and other fellows, carefully giving me feedback on my PRs. Hopefully they are happy with the contribution I and other fellows have made. Last but not least, The scope of work in Ethereum ecosystem is so vast that I have learned a lot from other fellows as well, e.g. on FOCIL implementation, PeerDAS, and some on SSZ optimization.

I plan to continue my work on the [programmable cryptography](tk) domain and bring privacy to blockchain users and improve the UX while using these privacy features.
