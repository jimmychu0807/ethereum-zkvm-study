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

4. Creating your own library
   - library couldn't have its own state
   - library functions must be marked internal. If not the library has to be deployed separately on-chain and cannot be embedded in the calling smart contract.

5. SafeMath
   - In solidity v0.8 and above, it adds integer overflow and underflow check. Not before.
   - To have it uncheck, you need an `unchecked {}` block.

6. Sending ETH in three different ways: `transfer`, `send`, `call`
   - `transfer`: has a limitation (feature) that can only use up to 2300 gas and it reverts any transaction that exceeds this gas limit.

     ```sol
     payable(msg.sender).transfer(amount); // the current contract sends the Ether amount to the msg.sender
     ```

   - `send`: has the limit of using up to 2300 gas. It returns false when failed instead of revert. Dev need to check and handle the failure case.

     ```sol
     bool success = payable(msg.sender).send(address(this).balance);
     require(success, "Send failed");
     ```

   - `call`: the most flexible way to send token and call any function.
     ```sol
     (bool success,) = payable(user).call{ value: address(this).balance }("");
     require(success, "Call failed");
     ```

7. For gas optimization
   - if possible, use `constant` and `immutable`. Avoid of storage read and write.
   - Revert with custom error instead of a error string.
     - error string - each character is a byte. Error will take 32 bytes + 32 bytes + strlen
     - custom error - error with no params are four bytes only.

8. `receive()` and `fallback()` functions
   - for a function to receive ether, it must be marked as `payable`.
   - `fallback()` if no function selector is matched, this function is triggered.
   - `receive()` if ether is sent over, this function is triggered.
   - this is the routing logic:
     - If `msg.data` >= 0, route to the function selector or `fallback()`. If `fallback()` doesn't exist in the target, it REVERT
     - If `msg.data` == 0 and `msg.value` > 0, solidity routtes to `receive()`. Or revert if `receive()` doesn't exist.
     - If `msg.data` == 0 and `msg.value` == 0, solidity routtes the call to `fallback()`.
