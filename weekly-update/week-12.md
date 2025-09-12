# Week 12 (Sep 1 - Sep 7) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

Compared to the past week:

- Completed the **execution** / emulation mode of Brevis Pico integration in grandine zkvm. In this mode the zkVM guest code is run without proof generation. We can benchmark the running time and execution cycles taken. Refer to [this section of the code](https://github.com/jimmychu0807/grandine-zk/pull/2/files#diff-940f069e4f5c73958e246678533e9a1bc81c972ef970f0eb16b33b49c27d62cbR64-R101).

- Completed the **prove** mode of Brevis Pico integration. This part will generate the zk proof on the guest code. Refer to [this section of the code](https://github.com/jimmychu0807/grandine-zk/pull/2/files#diff-940f069e4f5c73958e246678533e9a1bc81c972ef970f0eb16b33b49c27d62cbR103-R144).

- *wip*: Run benchmark of **execution** mode and **prove** mode on all four test cases in Grandine. Here is the [report so far](https://hackmd.io/@jimmychu0807/BkN45R09gl).
  - Pico panicked in the **mainnet-wo-epoch** test case with **Memory limit exceeded (0x78000000)**, while r0vm passes. Have [reported this issue](https://github.com/brevis-network/pico/issues/48) to pico team and is happy to assist pico team to investigate this issue.

  - Currently running **prove** mode in normal (cpu) mode and gpu mode for pico and r0vm and comparing the result.
