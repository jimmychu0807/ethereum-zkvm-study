# Week 14 (Sep 15 - 21) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

Compared to the past two weeks:

- Moved the PR code from [jimmychu0807/grandine-zk](https://github.com/jimmychu0807/grandine-zk/pull/2) to an official PR to the public Grandine repo - [PR #386](https://github.com/grandinetech/grandine/pull/386), with a [thorough comment](https://github.com/grandinetech/grandine/pull/386#issue-3451786488) inside. New code includes:
  - This code completed the [**zkvm/host/pico_helper**](https://github.com/grandinetech/grandine/pull/386/files#diff-478b88b7e4ee1a965391136ab8e534a9fe88058633c8ee07c6b56f2af295b7a1) so compiling the zkvm-host pico program (`cargo build -p zkvm_host --features pico`) will also compile the zkvm-guest pico program.
  - In **bls-zkcrypto** package code, added a new [`zkvm-pico` feature](https://github.com/grandinetech/grandine/pull/386/files#diff-b7052b91afd19d705c1fc1a239061689620d6a33549eb6f367fa450e21785eb2) to support running pico library that take a different parameter type on `hash_to_curve()` function in [secret_key.rs](https://github.com/grandinetech/grandine/pull/386/files#diff-bbad1fc544ca0bfa7f5dc97fac2339e2f3f233cb456210f00143e0f3244251af) and [signature.rs](https://github.com/grandinetech/grandine/pull/386/files#diff-d1beff702398e41d25d4f018be88937b50c7246e957523cc7d3e7a5139096f57).
  - Added a fix on [ssz/src/shared.rs](https://github.com/grandinetech/grandine/pull/386/files#diff-ec44286ad4cbc12cd03c34bb4cc367606de56aaf990c0c2e6b65d3bfe84f1c28) on the actual > maximum check on **read_list()**.

- Continue working on the [pico benchmarking report](https://hackmd.io/ubmyVWoBRueLDoEHXU1ZLw)
  - Ran the state transition function on r0vm gpu (bonsai) network, and added the benchmark number in **prove** mode.
  - Will collaborate with [Aman](https://github.com/0xprivateChaos) and [Ritesh](https://github.com/Dyslex7c) to add the benchmark numbers of different zkvms to this document.
