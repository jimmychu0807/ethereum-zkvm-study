## 2. ERC-721 NFT

Storing decentralized content
- IPFS directly
  - you have an option to rely on a centralized web server to access it.
- Using Pinata.cloud

- you can convert and svg image xml to a base64 encoding with `base64`

  ```bash
  base64 -i ./example.svg

  PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MDAiIGhlaWdodD0iNTAwIj4KICA8dGV4dCB4PSIyMDAiIHk9IjI1MCIgZmlsbD0iYmxhY2siPgogICAgSGkhIFlvdSBkZWNvZGVkIHRoaXMheyIgIn0KICA8L3RleHQ+Cjwvc3ZnPgo=
  ```

- Now the data is:
  ```
  data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI1MDAiIGhlaWdodD0iNTAwIj4KICA8dGV4dCB4PSIyMDAiIHk9IjI1MCIgZmlsbD0iYmxhY2siPgogICAgSGkhIFlvdSBkZWNvZGVkIHRoaXMheyIgIn0KICA8L3RleHQ+Cjwvc3ZnPgo=
  ```

- happy face base64 encoding
  ```
  data:image/svg+xml;base64,PHN2ZwogIHZpZXdCb3g9IjAgMCAyMDAgMjAwIgogIHdpZHRoPSI0MDAiCiAgaGVpZ2h0PSI0MDAiCiAgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIgo+CiAgPGNpcmNsZQogICAgY3g9Ijk2IgogICAgY3k9IjEwMCIKICAgIGZpbGw9InllbGxvdyIKICAgIHI9Ijc4IgogICAgc3Ryb2tlPSJibGFjayIKICAgIHN0cm9rZS13aWR0aD0iMyIKICAvPgogIDxnIGNsYXNzPSJleWVzIj4KICAgIDxjaXJjbGUgY3g9IjYxIiBjeT0iODIiIHI9IjEyIiAvPgogICAgPGNpcmNsZSBjeD0iMTI3IiBjeT0iODIiIHI9IjEyIiAvPgogIDwvZz4KICA8cGF0aAogICAgZD0ibTEzNi44MSAxMTYuNTNjLjY5IDI2LjE3LTY0LjExIDQyLTgxLjUyLS43MyIKICAgIHN0eWxlPSJmaWxsOm5vbmU7IHN0cm9rZTogYmxhY2s7IHN0cm9rZS13aWR0aDogMzsiCiAgLz4KPC9zdmc+Cg==
  ```

- low-level `call`, this is for transaction that change the state
- `staticcall`, this is for view or pure function.

## 3. Defi protocol

- AAVE, Compound - they are all lending protocol
- Uniswap - it is DEX

- On the commenting standard NATSPEC!
- Check-Effect-Interaction
  - Effect is updating the internal application states
  - interaction is calling external address or transferring value/tokens

- fuzz testing and stateful fuzz testing

- todo: actually good to follow doing section 3

- there is a `bound()` function that kind of like doing modulo on the size of valid input.

### On invariant testing

What `runs` and `depth` actually mean

From the invariant docs:
- `runs`: how many independent random sequences to try.
- `depth`: how many function calls are executed in each sequence (each run).

After every single call, all `invariant_*` functions are executed and must hold.

So one “campaign” is:

  For run in 1..runs:
    For step in 1..depth:
      - Pick a random target function (among all exposed via targetContract / targetSelector).
      - Fuzz its arguments.
      - Call it.
      - Immediately run that particular invariant function chosen in the campaign and check assertions.
    Move on to the next run, starting from the same initial setUp() state (i.e. each run starts fresh).

If a call reverts, the depth counter still increments, and invariants are still checked afterwards (unless fail_on_revert = true, in which case the test fails on the revert itself).

# Future Todo
* should learn the lending protovol well
