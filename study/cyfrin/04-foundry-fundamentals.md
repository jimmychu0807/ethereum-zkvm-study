- EIP-1559 (Fee Market Change)
  - https://eips.ethereum.org/EIPS/eip-1559
  - EIP-1559 changes how transaction fees are calculated, replacing the old bidding system with a dynamic "base fee" that automatically adjusts to network demand. It reduces gas price volatility, improve UX by eliminating the need to guess fees and introduces a deflationary mechanism where the base fee is "burned".
  - EIP-1559 transaction type, it is **0x02**.
  - Before EIP-1559 transaction type, it is **0x00**.

- EIP-2718 (Typed Transaction Envelope)
  - https://eips.ethereum.org/EIPS/eip-2718
  - `TransactionType || TransactionPayload`

- `vm.wrap()` - set the `block.timestamp`
- `vm.roll()` - set the `block.number`
- `vm.recordLogs()` - to record logs
- `vm.getRecordedLogs()` - retrieve the recorded logs
