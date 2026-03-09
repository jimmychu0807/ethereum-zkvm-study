# 01: smart contract study guide

## EIP-712: Typed structured data hashing and signing
- https://www.perplexity.ai/search/in-foundry-test-the-std-cheat-xR0gotrRR1.0XXIAJF.DhA#11
- there is a domain
- there is a type that define the message
- the message itself
- The user will then sign this struct.
- The

The domain includes the following:
```json
"EIP712Domain": [
  { "name": "name",    "type": "string"  },
  { "name": "version", "type": "string"  },
  { "name": "chainId", "type": "uint256" },
  { "name": "verifyingContract", "type": "address" }
]
```

The type that define the message
```json
{
  "types": {
    "EIP712Domain": [
      { "name": "name",    "type": "string"  },
      { "name": "version", "type": "string"  },
      { "name": "chainId", "type": "uint256" },
      { "name": "verifyingContract", "type": "address" }
    ],
    "Permit": [
      { "name": "owner",    "type": "address" },
      { "name": "spender",  "type": "address" },
      { "name": "value",    "type": "uint256" },
      { "name": "nonce",    "type": "uint256" },
      { "name": "deadline", "type": "uint256" }
    ]
  },
}
```

The message:
```json
"domain": {
  "name": "PermitToken",
  "version": "1",
  "chainId": 1,
  "verifyingContract": "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC"
},
"message": {
  "owner":   "0x1234000000000000000000000000000000000000",
  "spender": "0xABCD000000000000000000000000000000000000",
  "value":   "100000000000000000000",
  "nonce":   7,
  "deadline": 1710000000
}
```

## Pre-image Resistance

It is computationally infeasible to find an input message that produces a given hash output.
  L this is called pre-image resistance

It is computationally infeasible to find two different inputs that produce
the same hash output.
  L this is called collision resistance

## EIP-155 Replay Protection

Before EIP-155, what get signed are:

**hash(nonce, gasPrice, gasLimit, to, value, data)**

After EIP-155, what get signed are:

**hash(nonce, gasPrice, gasLimit, to, value, data, chainId, 0, 0)**

# 02: Core Solidity

## `delete` Usage

- can delete a single simple type value, storage
- For array
  - you can delete the whole array `delete myArr`
  - you can clear a single entry, `delete myArr[0]`, but the array length won't change
- For mapping
  - you cannot delete a mapping, `delete myMap` <- invalid, causing compilation error ❌
  - can clear an entry `delete myMap[0]`
- For struct
  - reset the struct var to its default values.

## Multiple Inheritance

- https://www.cyfrin.io/glossary/inheritance-solidity-code-example

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/

contract A {
  function foo() public pure virtual returns (string memory) {
    return "A";
  }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
  // Override A.foo()
  function foo() public pure virtual override returns (string memory) {
    return "B";
  }
}

contract C is A {
  // Override A.foo()
  function foo() public pure virtual override returns (string memory) {
    return "C";
  }
}

// Contracts can inherit from multiple parent contracts.
// When a function is called that is defined multiple times in
// different contracts, parent contracts are searched from
// right to left, and in depth-first manner.
contract D is B, C {
  // D.foo() returns "C"
  // since C is the right most parent contract with function foo()
  function foo() public pure override(B, C) returns (string memory) {
    return super.foo();
  }
}

contract E is C, B {
  // E.foo() returns "B"
  // since B is the right most parent contract with function foo()
  function foo() public pure override(C, B) returns (string memory) {
    return super.foo();
  }
}

// Inheritance must be ordered from “most base-like” to “most derived”.
// Swapping the order of A and B will throw a compilation error.
contract F is A, B {
  function foo() public pure override(A, B) returns (string memory) {
    return super.foo();
  }
}

contract G is A {}

contract H is B, G {
  // you don't put G below as G doesn't override foo()
  // this function returns "B"
  function foo() public pure override(B, A) returns (string memory) {
    return super.foo();
  }
}
```

this is quite tricky,
- in multiple inheritance if there are more than one function with the same name and param sets,
  you need to define an override to specify it.
- the right-most class with override the function impl of the left class
- the base class must be on the left and inherited class on the right.

# 03: defi & Apps Concepts

## Curve Stableswap Invariant

Curve’s StableSwap invariant is the pricing formula Curve v1 uses so that swaps between pegged assets (like stablecoins) have very low slippage when the pool is near balance, but still remain safe and self‑correcting when the pool becomes imbalanced.

Core idea
A pure constant‑sum AMM (e.g. x1 + x2 = K) gives almost zero slippage but can be completely drained of one token when large trades push it off balance.

A pure constant‑product AMM (e.g. x⋅y = K, like Uniswap v2) always keeps reserves on both sides, but slippage is relatively high even for small moves around the peg.

StableSwap hybridizes these: it behaves almost like constant‑sum around the equilibrium (1:1 peg) and smoothly morphs toward constant‑product as the pool gets more imbalanced.

In practice, this means:

Near 1:1, trades see tiny slippage (ideal for stablecoins).

As one side gets depleted, the price curve steepens like Uniswap, making further imbalance increasingly expensive and preventing the pool from being drained.

This is in Curve v1.

## MakerDAO stability fee

MakerDAO’s stability fee is the interest rate you pay on DAI debt when you open a Maker vault (borrow DAI), and it’s one of the main levers used to keep DAI close to 1 USD.

What it is, mechanically:
- When you lock collateral (e.g. ETH) and mint DAI, you create a debt position.
- On that outstanding DAI debt, you accrue a stability fee, quoted as an annual percentage rate (APY).
- The fee accrues over time on top of your principal; when you repay your vault, you repay:
  - The original DAI you minted, plus
  - The accumulated stability fee (effectively interest).

Implementation‑wise, the protocol accrues this via a per‑collateral rate that is periodically updated on‑chain (in the Jug/Vat modules in Maker’s architecture), so your debt grows continuously at that rate.

## ERC-4626 convertToAssets

- calculate teh amount of underlyasset given the vault share

- this is the function signature. Notice it is a view function.

  ```sol
  /**
   * @dev Returns the amount of assets that the Vault would exchange for the amount of shares provided, in an ideal
   * scenario where all the conditions are met.
   *
   * - MUST NOT be inclusive of any fees that are charged against assets in the Vault.
   * - MUST NOT show any variations depending on the caller.
   * - MUST NOT reflect slippage or other on-chain conditions, when performing the actual exchange.
   * - MUST NOT revert.
   *
   * NOTE: This calculation MAY NOT reflect the “per-user” price-per-share, and instead should reflect the
   * “average-user’s” price-per-share, meaning what the average user should expect to see when exchanging to and
   * from.
   */
  function convertToAssets(uint256 shares) external view returns (uint256 assets);
  ```

## Cliff

- it is the withholding period when no tokens are being released. Then the vesting schedule starts, and tokens are released.

## EIP-2612 `permit`
- allow accepting off-chain signature of an ERC-20 approve(), and perform the DEX operation in one tx.

## Liquidity Bootstrapping Pool

- it is an initial price discovery mechanism
- LBPs use time‑dependent weights: e.g. 90/10 → 50/50 over N hours, so price changes even if nobody trades, providing a built‑in “descending auction” effect.

## Vampire Attack

- A DEX using a higher yield to convince liqudity providers to move away from other DEXs to their own DEX.

## Timelock Contracts

To enforce a mandatory delay between when a proposal is passed and when its tx can be executed.

# 04: Foundry

- `cast index` to find the storage slot for a mapping
- Slither / Aderyn - static analysis tool
- There is a **differential testing** - that test on the different implementation of the same logic
- there is a `vm.snapshotState()` and `vm.revertToState(snpashotId)`
- When you want to do dry run on a state-changing function, use `cast call`.
