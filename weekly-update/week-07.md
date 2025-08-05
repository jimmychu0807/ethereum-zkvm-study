# Week 7 (Jul 28 - Aug 3) Update

## Quick Summary

1. Be able to get consensus-spec-tests test case to be run in Grandine with [Povilas's Grandine PR](https://github.com/grandinetech/grandine-zk/pull/1)
2. Investigated further on the above PR.
3. Started on Brevis Pico zkVM integration

## Details

### 1. Getting consensus-spec-tests to run

The main grandine-zk code is reading **SignedBeaconBlock** and **BeaconState** objects in and pass it in to zkVM `vm.execute()`, as shown below:

```rust
fn main() -> Result<()> {
    // ...
    let block_ssz = std::fs::read(Path::new(env!("CARGO_MANIFEST_DIR")).join(selected_test.block))?;

    let state_ssz = std::fs::read(Path::new(env!("CARGO_MANIFEST_DIR")).join(selected_test.state))?;

    let expected_root = {
        let block = SignedBeaconBlock::<Mainnet>::from_ssz(&config, block_ssz.clone())?;
        let mut state = BeaconState::<Mainnet>::from_ssz(&config, state_ssz.clone())?;

        state_transition(&config, &mut state, &block)?;
        state.hash_tree_root()
    };

    match args.command {
        Command::Execute => {
            let started_at = Instant::now();
            let vm = Vm::new()?;
            let (output_bytes, report) = vm.execute(state_ssz, block_ssz)?;
            let state = BeaconState::<Mainnet>::from_ssz(&config, output_bytes)?;

            println!("elapsed: {:?}", started_at.elapsed());
            println!("cycles: {}", report.cycles());

            println!("state slot after state transition: {}", state.slot());
            println!(
                "state root after state transition: {:?}",
                state.hash_tree_root()
            );
            assert_eq!(state.slot(), selected_test.expected_slot);
            assert_eq!(state.hash_tree_root(), expected_root);
        }
    }
    // ...
}
```

Inside consensus-spec-tests, the [`empty_block_transition` test case for electra](https://github.com/ethereum/consensus-spec-tests/tree/master/tests/mainnet/electra/sanity/blocks/pyspec_tests/empty_block_transition) has exactly a BeaconState (the `pre.ssz_snappy`) and SignedBeaconBlock (the `blocks_0.ssz_snappy`).

So grandine-zk should be able to take these two files, perform state transition, and generate proof.

But it is not the case, and after reported to Grandine team. Povilas worked out [a fix on PR#1](https://github.com/grandinetech/grandine-zk/pull/1) to make Grandine read the test case.

### 2. Investigation on the fix PR

The solution of the above PR is that Grandine code base used to determine the corresponding phase of a block from its slot number by parsing from a specific byte offset of a block ssz bytes, as shown in [the src](https://github.com/grandinetech/grandine-zk/blob/cc00cdd199dcc45df0217b7f286c1ba65d3fd9fb/types/src/combined.rs#L1603-L1607) or below:

```rust
impl<P: Preset> SszRead<Config> for SignedBeaconBlock<P> {
    fn from_ssz_unchecked(config: &Config, bytes: &[u8]) -> Result<Self, ReadError> {
        // There are 2 fixed parts before `block.message.slot`:
        // - The offset of `block.message`.
        // - The contents of `block.signature`.
        let slot_start = Offset::SIZE.get() + SignatureBytes::SIZE.get();
        let slot_end = slot_start + Slot::SIZE.get();
        let slot_bytes = ssz::subslice(bytes, slot_start..slot_end)?;
        let slot = Slot::from_ssz_default(slot_bytes)?;
        let phase = config.phase_at_slot::<P>(slot);
        //...
    }
}
```

But this is not the case for the consensus-spec-tests test case. With the fix, we can now specify the phase in **ContainerParams**.

```rust
#[derive(Clone, Copy, Default, Debug)]
struct ContainerParams {
    phase: Option<Phase>,
    decompress_snappy: bool,
}
```

Once specified the right phase parameter, Grandine could successfully parse the SignBeaonBlock and BeaconState and perform state transition.

During the investigation, I also made a detour to learn about using [rust-lldb](https://lldb.llvm.org/) and how to use it to step through rust code. Study note is made here: https://hackmd.io/@jimmychu0807/r15Xq-Dvex

A key observation is that it has GUI mode that simplify a lot of the rust-lldb handling. Just type `gui` in the lldb prompt **after putting a breakpoint and executing the program**.

### 3. Brevis Pico zkVM integration

Went through Brevis Pico [quick start](https://pico-docs.brevis.network/getting-started/quick-start). One issue is that to compile the quick start template, it is [pinned](https://github.com/brevis-network/pico-zkapp-template/blob/main/rust-toolchain) to `nightly-2024-11-27` rust tool-chain, while Grandine uses a more updated stable rust toolchain.

I need to investigate how to run Brevis Pico and Grandine with a rust toolchain that's compatible to both repository.

## Next Step

1. Integrate Brevis Pico into Grandine-zk
2. Find a rust toolchain that is compatible to both Grandine-zk and Brevis Pico.
