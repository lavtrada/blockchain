# Blockchain Lottery

A simple lottery smart contract written in Solidity (`0.8.0`).

## How it works

- **Manager**: whoever deploys the contract becomes the `manager`.
- **Entering**: players join by sending exactly **0.1 ETH** straight to the contract address (handled by `receive()`). Each payment adds the sender to `players`.
- **Balance**: `getBalance()` returns the contract's balance. Only the manager can call it.
- **Picking a winner**: the manager calls `pickWinner()` once there are **at least 3 players**. A pseudo-random index picks the winner, who gets the whole balance. The player list is then reset for the next round.

## Contract overview

| Member | Type | Description |
|---|---|---|
| `players` | `address payable[]` (public) | Current round's entrants |
| `manager` | `address` (public) | Contract deployer |
| `receive()` | external payable | Enter the lottery with 0.1 ETH |
| `getBalance()` | public view | Contract balance (manager only) |
| `random()` | internal view | Pseudo-random number from `block.difficulty`, `block.timestamp`, `players.length` |
| `pickWinner()` | public | Pays the winner and resets the round (manager only) |

## Running it (Remix)

1. Open [Remix IDE](https://remix.ethereum.org) and create `Lottery.sol` with this repo's code.
2. Compile with Solidity compiler **0.8.0**.
3. Deploy with the "Remix VM" environment. The deploying account is the manager.
4. Switch to other accounts, set value to `0.1 ether`, and use **Transact** (low-level interaction) to send ETH to the contract.
5. Switch back to the manager account and call `pickWinner()`.

## Notes

- `getBalance()` has a typo (`manage`, smart quotes, stray `r`) that stops it compiling. It should be:
  ```solidity
  require(msg.sender == manager, "You are not the manager");
  ```
- The randomness can be predicted and influenced by miners/validators. It's fine for learning, not for real money. Use something like Chainlink VRF in production.

## License

GPL-3.0
