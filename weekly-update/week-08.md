# Week 8 (Aug 4 - Aug 10) Update

Refer to the PR:
https://github.com/jimmychu0807/grandine-zk/pull/1

1. Pinned grandine-zk to toolchain [nightly-2024-11-27](https://github.com/jimmychu0807/grandine-zk/pull/1/files#diff-2b1bde2cf3a858b7bf7424cb8bcbf01f35b94dc80b925d9432cbab3319ca9b4e) due to Brevis Pico pinning.
2. Refactored `host/src/backend.rs` to becomes `host/src/backend/mod.rs`, `host/src/backend/sp1.rs`, `host/src/backend/risc0.rs`
3. Added host code of [`host/src/backend/pico.rs`](https://github.com/jimmychu0807/grandine-zk/pull/1/files#diff-940f069e4f5c73958e246678533e9a1bc81c972ef970f0eb16b33b49c27d62cb)
4. Added guest code at [`guest/pico/*`](https://github.com/jimmychu0807/grandine-zk/pull/1/files#diff-b622fd118817f3bd6bf7b754cca00afa983d87cc40544a6d0cfd144fe7548529) - encountered an error that building the guest code with `cargo pico` would return a linker error (something about `ll`).

Next week todo:

Continue to resolve #4, and then check that the host and guest code work together.
