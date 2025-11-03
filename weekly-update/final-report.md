# Report on Brevis Pico zkVM integration & benchmarking on block state transition function in Grandine

<!-- This final update is meant to be understandable by anyone who wants to learn about have you done in the cohort without diving into your project proposal and every update. Use it as a summary about current state of things with all relevant information and links. -->

## Project Abstract

Grandine is an Ethereum client in both executive and consensus layer written in Rust. As Ethereum has an initiative of moving to use zkVM to generate and verify proof instead of reduplicating the computation effort of running the state transition function. Grandine team have written an extension framework that could easily integrate any kind of zkVM to benchmak its stf performance. It is stored inside Grandine main repository [`zkvm` directory](https://github.com/grandinetech/grandine/tree/develop/zkvm).

Using this extension framework, Grandine team have already integrated [**r0vm**](https://github.com/grandinetech/grandine/tree/develop/zkvm/guest/risc0) and [**SP1**](https://github.com/grandinetech/grandine/tree/develop/zkvm/guest/sp1) zkVMs and now want to integrate with other zkVMs.

As there were a few EP fellows working on this initiative, each of us was assigned to integrate one zkVM.

- [me](tk:github): [Brevis Pico](tk)
- [Ritesh](tk:github): [ZKM Ziren](tk)
- [Aman](tk:github): [Zisk](tk)

In the following sections, I will detail out how I have integrated Brevis Pico zkvm in Grandine, refactored the zkvm extension to be more modular, initiated to add one more test case to run, and attempt in running the actual zkvm proof generation in both CPU machine locally and GPU machine via Bonsai Network.

## Detail Work & Final State

### Main zkvm host and guest code

This part is updated via Grandine [main repo PR #386](https://github.com/grandinetech/grandine/pull/386).

For zkVM integration, there is a host and guest section. The [host code](https://github.com/grandinetech/grandine/blob/develop/zkvm/host/src/main.rs) performs the following functionality:

- Based on the test case selected from cli argument, get the 1) chain/phase config, 2) Signed Beacon Block in [SSZ format](tk), 3) Beacon State in SSZ format.

- Load the public key caching for performance optimization.

- Call the state transition function with the four input paramters of the config, signed beacon block, beacon state, and cache to get the final state.

- Then, the host initiate the zkvm runtime and pass in the four input arguments.

- Finally, the host retrieves the final state root computed from the zkvm runtime and compare it with the computation by itself and check (assert) if they are the same.

The **guest code** is run inside the zkvm runtime. It performs the following functionality:

- It retrieve and parse the four input arguments from the host, deserializing the object from SSZ format.

- Run the state transition based on the four inputs, and compute the result. It may or may not generate the proof depending on whether it is run in execute mode or prove mode.

- Result the final state and possibly with the proof back to the host.

There are two modes when running the guest, **Execute** mode and **Prove** mode. Execute mode will only execute the instructions in zkvm runtime and return the result. Prove mode will also generate the zk-proof and return it to the host in addition to the result. As a developer, we can see execute mode to check the computation is consistent with what's computed with its CPU counterpart, while the proof generation is what we are really interested in and the most time-consuming step.

### Main Grandine PR#386

First, I have refactored the `zkvm/host` so each VM is stored in a separate module (e.g. [risc0](tk) [sp1](tk)). Then I added my implementation of [Pico host code](https://github.com/grandinetech/grandine/pull/386/files#diff-1d8fb10e7eb152891f7c37b4a249dc4092ff33434189694ee7aee7d77cfa5a9e) inside. It is more or less the same as the code of other zkvm and implement the necessary trait calling from the specific zkvm API.

A [build helper](https://github.com/grandinetech/grandine/pull/386/files#diff-2015ba7f1b0aad9904659f52f2844d91f022c63a2b10db135b2332668943a4a0) is written so that building the zkvm-host code will also trigger building the zkvm guest code. This is convenient to users or else need to run two separate commands to compile both the host and guest code. Care need to be taken that Pico requires building with nightly rust version, while the main Grandine repo run with stable rust version. So all pico related build and execution command must be run with prefix `cargo +nightly-2025-08-04 ...`.

The [zkvm guest code](https://github.com/grandinetech/grandine/pull/386/files#diff-b622fd118817f3bd6bf7b754cca00afa983d87cc40544a6d0cfd144fe7548529) for Pico is also similar to the guest code for other zkVM, but just calling Pico-specific zkvm guest API.

Lastly I also added a github CI workflow script [`zkvm-test.yml`](https://github.com/grandinetech/grandine/pull/386/files#diff-3100b4a9081a2d21a9a64759e0d35e33684fac9cb222c56d8c00135f23beaed6) so there is a CI test to build different zkVM host and guest code and run two test cases. This serves as a sanity check to ensure that future code change doesn't break the zkVM host and guest code.

The work is enclosed in [PR#386](https://github.com/grandinetech/grandine/pull/386).

### Accompanied PRs to SHA2 and BLS12-381

Now the state transition function heavily relies on SHA256 hash function and BLS12-381 elliptic curve based cryptography. Each zkVM has their own tweaks and optimization on running the hash function and elliptic curve arithmetics.

Grandine has a universal-precompile packages of [SHA256](https://github.com/grandinetech/universal-precompiles/tree/RustCrypto-hashes/sha2-v0.10.9) and [BLS12-381](https://github.com/grandinetech/universal-precompiles/tree/zkcrypto/bls12_381-6bb9695) for each zkVM based on the compilation feature and target.

To support Brevis Pico, I have to patch with Brevis Pico speciality for these two libraries:

- [SHA256 PR#3](https://github.com/grandinetech/universal-precompiles/pull/3) and
- [BLS12-381 PR#4](https://github.com/grandinetech/universal-precompiles/pull/4)

The BLS12-381 PR is particularly delicate as there are more than 50 places that need to apply unique computation based on a specific zkVM. Brevis Pico also have a `target_vendor` of **risc0** which is the same to r0vm, so for Pico we need to add an additional feature flag **zkvm-pico** during compilation to target Pico zkvm. We then also add a **zkvm-risc0**feature flag to target r0vm.

### Benchmark Running

NX> on running the benchmark in cpu and gpu cluster

## Current Impact and Future of the project

Potential next steps and improvements, use cases in the ecosystem

## Self-evaluation

- general feedback about your project and activity within EPF
- Be realistic and reflect on your progress, achievements, your satisfaction and criticism
