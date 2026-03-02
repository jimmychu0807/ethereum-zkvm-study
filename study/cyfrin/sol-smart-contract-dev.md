# Solidity Smart Contract Development

url: https://updraft.cyfrin.io/courses/solidity

## 1.8 Functions

visibility
- public: can be called as external function, also available to the contract itself
- private: accessible from the contract itself
- external: can be called as external function, but not available to the contract itself
- internal: accessible from the contract and contract derived from it.

## 1.11 Memory Storage and Calldata

Solidity store data in **SIX** different locations:
- calldata
- memory
- storage
- stack
- code
- logs

calldata vs memory:
- both are temporary storage locations
- calldata is used for function input parameters, can't be modified
- to modify calldata, it must first load to memory
- calldata is cheaper than memory

## 3. Fund Me

1. 1 ether = 1 x 10^9 Gwei = 1 x 10^18 wei

2. A transaction cover the following fields:
   - nonce: the account transaction counter
   - gas price (wei): how much price for each gas quantity used
   - gas limit: amount of gas sender is willing to use
   - to: recipient address
   - value: wei
   - Data: empty or the calldata (function and parameter)
   - v,r,s: components of the transaction signature

When a revert happens, all state changes are undone. Used gases are not refunded, but remaining gas is.

3. Chainlink offering

   - oracle
   - VRF - verifiable random function
   - chainlink automation - when a certain event is emitted, it will call certain function of a smart contract.
   - chainlink function
