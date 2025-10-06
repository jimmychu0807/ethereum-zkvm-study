# Week 16 (Sep 29 - Oct 5) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

In the past two weeks, I polished on [Grandine PR #386](https://github.com/grandinetech/grandine/pull/386). Grandine team Artiom has given me feedback on the PR. The first round of revision is done and I have addressed all the feedback. Major revision includes:

- An accompanied PR on integrating [Brevis Pico **sha2** tweak](https://github.com/brevis-network/hashes/tree/master/sha2) to Grandine universal-precompile. Refer to [Universal Precompile PR #3](https://github.com/grandinetech/universal-precompiles/pull/3).
- An accompanied PR on integrating [Brevis Pico **bls12_381** tweak](https://github.com/brevis-network/bls12_381/tree/pico-patch-v1.0.1-bls12_381-v0.8.0) to Grandine universal-precompile. Refer to [Universal Precompile PR #4](https://github.com/grandinetech/universal-precompiles/pull/4).
- [Added cargo features of `zkvm-risc0` and `zkvm-pico`](https://github.com/grandinetech/grandine/pull/386/files#diff-b9a966a6766d089f8f3954f894313fd1f550d596d3e5d2ecacf8f895fcd89c74) in Grandine bls-zkcrypto library to select the right crates based on running zkvm.

  Pico has a target string of **riscv32im-risc0-zkvm-elf** also. So this rust condition:

  ```rs
  #[cfg(all(target_os = "zkvm", target_vendor = "risc0"))]
  ```

  which is used to distinguish for r0vm previously will now match both r0vm and pico. This is now updated to, for r0vm:

  ```rs
  #[cfg(all(target_os = "zkvm", target_vendor = "risc0", feature = "zkvm-risc0"))]
  ```

  and for pico:

  ```rs
  #[cfg(all(target_os = "zkvm", target_vendor = "risc0", feature = "zkvm-pico"))]
  ```

- Fix most of the warning messages.

The development is quite delicate as Grandine universal-precompile contains the sha2 and bls12_381 tweaks of r0vm, sp1, Pico, and Ziren. So there are different conditions being built up to include/exclude different pieces of code. See [an example of `scalar.rs` here](https://github.com/grandinetech/universal-precompiles/pull/4/files#diff-964444a5f6bb8223a41b83081259a7ef041cc9ecc940bcc29b990e6afa5d8d30).

Tests has been all passed for r0vm, sp1, and pico zkvm.

### Next Todo

- Will continue to work with Artiom to get the PR merged. May have a 2nd round of revision.
- There are PR sequencing sync issue with Ritesh [PR #385](https://github.com/grandinetech/grandine/pull/385). Will resolve code conflict if PR #385 is merged first.
- Collaborate with [Aman](https://github.com/0xprivateChaos) to get Zisk zkvm integration done.
