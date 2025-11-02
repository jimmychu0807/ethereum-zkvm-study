# Week 20 (Oct 20 - Nov 2) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

The past two weeks, I was continuously following up with Grandine team on the three PRs:

- main one: [PR #386](https://github.com/grandinetech/grandine/pull/386)
- sha2: [PR #3](https://github.com/grandinetech/universal-precompiles/pull/3)
- bls12_381: [PR #4](https://github.com/grandinetech/universal-precompiles/pull/4)

After some dialogs, I have [unified bls12_381 hash_to_curve interface for `zkvm-pico` and non `zkvm-pico` interface](https://github.com/grandinetech/universal-precompiles/pull/4/files#diff-c803360fa20310b739bd8b045c4f9773b1ac8d6af2b6896393082a96c6ac221f).

Previously, the interface was:

```rs
pub trait HashToField: Sized {
    // ... existing code

    #[cfg(feature = "zkvm-pico")]
    fn hash_to_field<X: ExpandMessage>(message: &[u8], dst: &[u8], output: &mut [Self]) {
        let len_per_elm = Self::InputLength::to_usize();
        let len_in_bytes = output.len() * len_per_elm;
        let mut expander = X::init_expand(message, dst, len_in_bytes);

        let mut buf = GenericArray::<u8, Self::InputLength>::default();
        output.iter_mut().for_each(|item| {
            expander.read_into(&mut buf[..]);
            *item = Self::from_okm(&buf);
        });
    }

    #[cfg(not(feature = "zkvm-pico"))]
    fn hash_to_field<X, M>(message: M, dst: &[u8], output: &mut [Self])
    where
        X: ExpandMessage,
        M: Message,
    {
        let len_per_elm = Self::InputLength::to_usize();
        let len_in_bytes = output.len() * len_per_elm;
        let mut expander = X::init_expand::<M, Self::XofOutputLength>(message, dst, len_in_bytes);
        let mut buf = GenericArray::<u8, Self::InputLength>::default();
        output.iter_mut().for_each(|item| {
            expander.read_into(&mut buf[..]);
            *item = Self::from_okm(&buf);
        });
    }

    // ... other code
}
```

Now it is unified to become:

```rs
pub trait HashToField: Sized {
    // ... existing code
    fn hash_to_field<X, M>(message: M, dst: &[u8], output: &mut [Self])
    where
        X: ExpandMessage,
        M: Message,
    {
        let len_per_elm = Self::InputLength::to_usize();
        let len_in_bytes = output.len() * len_per_elm;
        let mut expander;

        #[cfg(feature = "zkvm-pico")]
        {
            use alloc::vec::Vec;
            let mut tmp = Vec::new();
            message.input_message(|m| tmp.extend_from_slice(m));
            expander = X::init_expand(&tmp[..], dst, len_in_bytes);
        }

        #[cfg(not(feature = "zkvm-pico"))]
        {
            expander = X::init_expand::<M, Self::XofOutputLength>(message, dst, len_in_bytes);
        }

        let mut buf = GenericArray::<u8, Self::InputLength>::default();
        output.iter_mut().for_each(|item| {
            expander.read_into(&mut buf[..]);
            *item = Self::from_okm(&buf);
        });
    }

    // ... other code
}
```

This change has somewhat simiplified codes that interface with bls12_381 in main Grandine repository (**bls-zkcrypto**).
