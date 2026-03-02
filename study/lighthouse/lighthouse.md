## starting local devnet in lighthouse repo
To start a local-testnet
1. either use kurtosis and change the config file
  https://github.com/ethpandaops/ethereum-package
  L this is lot easier to tweak the start_local_network script and ethereum package!!
  L can you tweak the Docker file so you build outside and just copy the binary inside the docker image?
    L to build lighthouse from @1337 -> @1415
    L verified what chong-he said that it stuck at the tokio part

2. or follow this readme (has already removed):
  https://github.com/sigp/lighthouse/blob/020702f/scripts/local_testnet/README.md
  L it is not recommended to run bootnode in macOS

## 251210 study

- client diversity:
  https://clientdiversity.org/#distribution

- lighthouse architecture overview:
  https://www.youtube.com/watch?v=pLHhTh_vGZ0

- lighthouse book:
  https://lighthouse-book.sigmaprime.io/

- **consensus** block
  - is where there is state transition
  - a lot of testing code is here

- great resource: https://eth2book.info/
