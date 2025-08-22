# Week 10 (Aug 18 - Aug 24) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

Compared to the past week:

- Referring to the work [jimmychu0807/grandine-zk#develop](https://github.com/jimmychu0807/grandine-zk/tree/develop), updated [risc0-zkvm](https://github.com/jimmychu0807/grandine-zk/commit/90cd578595e39a05683292fc0435fd391abc525f) and risk0-build crates from v2.3.1 to v3.0.1. This fixed an issue that compiling guest code complaining using an unstable language feature `unsigned_is_multiple_of` due to [this line of `dividend.is_multiple_of(...)`](https://github.com/jimmychu0807/grandine-zk/blob/90cd578595e39a05683292fc0435fd391abc525f/helper_functions/src/predicates.rs#L182).

- Updated Brevis Pico to latest v1.1.7 (`pico-sdk` = v1.1.7). Refer to work [jimmychu0807/grandine-zk#zkvm-pico](https://github.com/jimmychu0807/grandine-zk/tree/zkvm-pico). I could compile the zkvm host code and guest elf code. Here is [the instruction (hackmd doc)](https://hackmd.io/@jimmychu0807/BkwBuoSFle).

- Currently the problem is that proving for empty block transition takes 1.5hrs+ and still running (I killed the process, refer to the log inside my hackmd doc). I don't know if I am on the right track but just the process take a long long time to run, or somewhere there is something wrong in the code. Can't tell.

  Fyi, even for risc0 VM, the most stable zkVM I regard, generating a real proof with `RISC0_DEV_MODE=0` on empty block transition takes 2 hrs+ and still running, I killed the process also.

  I am checking with Grandine teammates see if they could shed any insight.
