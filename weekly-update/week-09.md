# Week 9 (Aug 11 - Aug 17) Update

## Brevis Pico zkVM integration & benchmarking on block state transition function

Compared to the past week:

I finish coding the host and guest side for pico zkvm. Then I run the tiny [consensus-spec-test case](https://github.com/jimmychu0807/grandine-zk/blob/f35552cc647a8f34dd789ccbf5b7490d5580db97/zkvm/host/src/main.rs#L133). It takes 30+ mins, and then the process got killed by the OS (Killed: 9) - maybe oom. It dies when deserializing the ssz_bytes back to BeaconState inside the zkvm...

I will check with Brevis Pico team on this issue...
