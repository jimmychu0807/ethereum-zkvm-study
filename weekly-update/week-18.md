# Week 18 (Oct 13 - 19) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

I am mainly waiting for Grandine team to perform the next iteration of review on [Grandine PR #386](https://github.com/grandinetech/grandine/pull/386), and two accompanied PRs: [sha2](https://github.com/grandinetech/universal-precompiles/pull/3) and [bls12_381](https://github.com/grandinetech/universal-precompiles/pull/4).

In the past week, I also wrote [a github workflow task]((https://github.com/grandinetech/grandine/pull/386/files#diff-3100b4a9081a2d21a9a64759e0d35e33684fac9cb222c56d8c00135f23beaed6)) to test build the zkvm host and guest code to ensure the zkvm-related codebase doesn't break in future development, or at least be able to detect it. Now the workflow will test for these zkvms:

- r0vm
- Succinct SP1
- Brevis Pico

I have to use [easimon/maximize-build-space](https://github.com/easimon/maximize-build-space) github ci action to reconfigure the disk allocation of the default github workflow container so there is enough disk space to install various zkvms and run the tests.

The task builds both the host and guest code and perform exeuction (without proof generation) to ensure the blockchain stf results run in zkvm and in regular CPU are the same.
