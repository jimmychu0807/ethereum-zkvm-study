# On Uniswap
Very good tut: https://uniswapv3book.com/

- uniswap v3 allows for limit order, when specifying different price range
- In ERC20, if you are going to use `transferFrom()`, you have to approve it, even for the owners themselves. Because `transferFrom()` impl simply just check the **allowance** mapping if the value inside is larger than the transferred value.

## Learning on flash loan
- Both Uniswap V2 and V3 implement flash loans: unlimited and uncollateralized loans that must be repaid in the same transaction. Pools give users arbitrary amounts of tokens that they request, but, by the end of the call, the amounts must be repaid, with a small fee on top.
- flash loan can only be taken by smart contract

- flash loan mostly for arbitrage
  - across different AMM/DEX
  - use flash loan to repay undercollatoralized position in Aave/compound
  - leverage or yield opt.

## M3 Cross-tick swaps
- you have to aggregate all the different pools for a pair (can be different tickSpacing)
- when a tick liqudity is depleted, move one tick, thus the price change, and check for liquidity, and fill up some more of the order.

## M4 Multi-pool swaps (or multi-hop swaps)
- CREATE to deploy contract, deployed addr determined by: `KECCAK(deployer.addr, deloyer.nonce)`
- CREATE2 to deploy contract, deployed addr determined by: `KECCAK(deployer.addr, salt, contractCodeHash)`
- uniswap v3 uses (token0, token1, tickSpacing) to distinguish a pool
  - these three values: `salt: keccak256(token0, token1, tickSpacing)` determines the deployment salt

AutoRouter
  - this is built on the client side, to find how to traverse from src tokens to dest tokens
  - you build a graph of tokens. Each token is a node. Each pair is a path between the two nodes. After loading up the pool pair, use a graph traversing algo to find the shortest path.
  - we are using the **tickSpacing** as the cost/distance of the path.

## M5 Fee and Price Oracle
There are multiple type of fee
- swap fee
- flash loan fee
- protocol fee: currently the protocol fee is distributed

Uniswap also functions as an on-chain price oracle
  - it stores the last 65535 tick of prices, so to some amount of historical prices
  - it calc. historical time-weighted geometric price.
  - the cardinality: it is the historical price to store for computation. Some one has to pay for this to expand a token cardinality.

## M6 position represented as NFT
- they use svg to render a graphics with JSON data. svg can be composed with textual data and be shown. JSON to display the actual key data/information.
- when you withdraw all from a pool, the NFT is burnt.

# On Vault

## ERC-4626: tokenized vault standard
- https://eco.com/support/en/articles/12068953-understanding-erc-4626-the-complete-guide-to-tokenized-vault-standard
- https://ethereum.org/developers/docs/standards/tokens/erc-4626/

conversion between asset <-> vault share

- it is inherited from ERC-20

## ERC-7540: asynchronous vault
- rwa
- cross-chain lending

## ERC-7575 multi-asset vaults

# On smart contract

## call, delegatecall(), and staticcall()

Native solidity

- addr.call{value: ..., gas: ...}(data)
- addr.delegatecall(data)
- addr.staticcall(data)

- **call()** is the most generic way of calling external function
- **delegatecall()** - it uses the current smart contract context
- **staticcall()** - it enforces the read-only call()

- **staticcall()** is not for estimate gas cost, but to get the function return value. Solidity compile external call to view function using staticcall()

- Remember, what call() return is:

  ```sol
  (success, result) = target.call{value: msg.value}(data);
  ```

  - it is not just the result returned from the call.

### Openzepillin helpers

Openzepellin **Address** library has a helper function as **Address.functionCall(target, data)**.
- it ensures target is a contract

It also has:
- Address.functionCall(target, data)
- Address.functionCallWithValue(target, data, value)
- Address.functionStaticCall(target, data)
- Address.functionDelegateCall(target, data)

## On calling in low level way and retrieving the return value
```sol
// Use abi,encodeWithSignature() for calldata composition
bytes memory data = abi.encodeWithSignature("enter(bytes8)", bytes8(_gatekey));

// It will always return even when the external function revert
// If the call revert, the result will be `0x`
// So if the caller may revert, you should check `success` flag first before decoding the result as
//   the expected return value. The result might be 0x if the caller revert.
(bool success, bytes memory result) = address(gk).call{gas: gas}(data);

if (success) {
    (bEnter) = abi.decode(result, (bool));
    ...
}
```

## On some low level call

you can determine if it is calling from an EOA or smart contract by:

```sol
uint256 x;

// One or the other
assembly {
  x := extcodesize(msg.sender)
}
// or
x = msg.sender.code.length == 0

// Then check
require(x == 0);
```

```sol
address addr;
assembly {
  addr := caller()
}

require(msg.sender == addr);
```

## Special property of making external call during constructor phase

While contract A’s constructor is running, its code is not yet deployed, so in contract B the value of msg.sender.code.length will be 0, not the final code size of A.

Reasoning:
- A contract’s runtime bytecode is only stored at its address after the constructor finishes. Until then, the address exists but has no code.
- If A’s constructor calls B, then inside B:
  - msg.sender is A’s address.
  - But msg.sender.code.length uses the runtime code at that address, which is still empty during A’s construction, so it returns 0.
