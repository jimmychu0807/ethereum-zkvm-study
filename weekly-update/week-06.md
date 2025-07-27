# Week 5 (Jul 21 - 27) Update

## Quick Summary

- Read the Grandine zk-vm source code and have a more concrete picture on what need to be done in this project
- Be able to run Grandine zkvm existing test cases and made some benchmarking
- Studied about SimpleSerialize format and figured out how to decompress ssz_snappy test case to inspect the data inside

## Details

### What need to be done in this project

The core of Grandine zkVM project revolves around getting **block_ssz**, **state_ssz**, and **cache** objects, and passing them into the zkVM for processing and getting the proof/receipt back. The state transition functions have already been implemented, it seems.

```rs
let block_ssz = get_or_download(/*...*/)?;
let state_ssz = get_or_download(/*...*/)?;

let (expected_root, cache) = {
    let block = SignedBeaconBlock::<Mainnet>::from_ssz(&config, block_ssz.clone())?;
    let mut state = BeaconState::<Mainnet>::from_ssz(&config, state_ssz.clone())?;
    let cache = PubkeyCache::load(Database::in_memory());

    // performing on the host to get the cache object that speed up the zkVM processing
    state_transition(&config, &cache, &mut state, &block)?;

    (state.hash_tree_root(), cache.to_ssz().unwrap())
};

//...
let vm = Vm::new()?;
let (output_bytes, report) = vm.execute(state_ssz, block_ssz, cache)?;
let state_root = H256(output_bytes.try_into().unwrap());
```

- **block_ssz** - is in `SignedBeaconBlock` type
- **state_ssz** - the `BeaconState` type
- **cache** - is the caching of some bls-key computations to speed up zkVM processing.

`Vm` is the zkVM object that implements [`VmBackend` trait](https://github.com/grandinetech/grandine-zk/blob/cc00cdd199dcc45df0217b7f286c1ba65d3fd9fb/zkvm/host/src/backend.rs#L15) and specified differently based on the feature flag during compilation. Currently feature flag of `risc0` and `sp1` are supported, corresponding to r0vm and sp1 zkVM.

What I need to do for the project is to code a new object that implements `VmBackend` trait and instantiate Brevis Pico zkVM.

### Grandine zkvm benchmarking

The following are the benchmarking result of running test cases that has already been implemented in Grandine-zk code base on my local Macbook Air (2020 M1 chip).

1. `pectra-devnet-6 without epoch transition`
   - **RISC0_DEV_MODE=1** (dev mode)
     - cmd:
       ```
       RISC0_DEV_MODE=1 RUST_LOG=info ./target/release/zkvm_host --test "pectra-devnet-6 without epoch transition" execute
       ```
     - result:
       ```
       INFO risc0_zkvm::host::server::exec::executor: execution time: 233.025777s
       read input:        443847879
       state transition: 3878333922
       write output:         196300
       elapsed:             204.54s
       cycles:           4919394304
       ```
       Take about **3m20s**.

   - **RISC0_DEV_MODE=0** (generating a real proof)
     - cmd:
       ```
       RISC0_DEV_MODE=0 RUST_LOG=info ./target/release/zkvm_host --test "pectra-devnet-6 without epoch transition" execute
       ```
     - result:
       *cancelled after running for more than 4 hours*

2. `pectra-devnet-6 with epoch transition`
   - **RISC0_DEV_MODE=1** (dev mode)
     - cmd:
       ```
       RISC0_DEV_MODE=1 RUST_LOG=info ./target/release/zkvm_host --test "pectra-devnet-6 wit epoch transition" execute
       ```
     - result:
       ```
       INFO risc0_zkvm::host::server::exec::executor: execution time: 1565.041949055s
       read input:         443289185
       state transition: 35890144311
       write output:          177150
       elapsed:             1565.34s
       cycles:           39242432512
       ```

       Take about **26m**.

   - **RISC0_DEV_MODE=0** (generating a real proof)
     *don't bother to run locally*

3. `mainnet without epoch transition`
   - **RISC0_DEV_MODE=1** (dev mode)
     - cmd:
       ```
       RISC0_DEV_MODE=1 RUST_LOG=info ./target/release/zkvm_host --test "mainnet without epoch transition" execute
       ```
     - result:
       *cancelled after running for more than 4 hours*

   - **RISC0_DEV_MODE=0** (generating a real proof)
     *don't bother to run locally*

### On SimpleSerialize and decompressing ssz_snappy test file.

To read the test state in [consensus-spec-tests](https://github.com/ethereum/consensus-spec-tests), somehow [snzip](https://github.com/kubo/snzip) doesn't work for me to decompress ssz_snappy to ssz file. I need to write a [small wrapper script](https://hackmd.io/@jimmychu0807/grandine-zkvm-study-250725#About-SimpleSerialize) with [**python-snappy**](https://pypi.org/project/python-snappy/) in order to decompess ssz_snappy to ssz. Then I can inspect the data on https://simpleserialize.com/, by uploading the ssz file and specifying the right *fork* and *SSZ type*.

## Next Step

1. Find 2-3 really small test states from [**consensus-spec-tests** repo](https://github.com/ethereum/consensus-spec-tests), or construct them my own, to feed as the **block**, and **state** in the zkVM so a proof can be generated in a reasonably short amount of time locally (say in 5 - 10 mins). Then it will be used as the basis of a sanity check when implementing new zkVM object.

2. Implement a new zkVM object that intantiate Brevis Pico zkVM, and have the snaity check pass.

## Reference
- [Group Project Proposal PR](https://github.com/eth-protocol-fellows/cohort-six/pull/199)
- [Grandine zkVM study note 25-07-24](https://hackmd.io/@jimmychu0807/grandine-zkvm-study-250724)
- [Grandine zkVM study note 25-07-25](https://hackmd.io/@jimmychu0807/grandine-zkvm-study-250725)
