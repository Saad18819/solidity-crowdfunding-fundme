# Decentralized Crowdfunding Smart Contract

A secure, gas-optimized crowdfunding smart contract built from scratch in Solidity. This project connects to live decentralized networks using Chainlink Oracles to handle real-world asset conversions dynamically.

## Features
- **Live Price Feeds:** Integrates Chainlink's `AggregatorV3Interface` to fetch the real-time price of ETH/USD on the Sepolia Testnet.
- **Dynamic Funding Minimums:** Enforces a strict minimum donation limit ($5 USD) regardless of market volatility.
- **Reentrancy Protection:** Built securely using the **CEI (Checks, Effects, Interactions)** pattern to prevent flash loan and draining exploits.
- **Native Callbacks:** Equipped with `fallback()` and `receive()` functions to ensure users can fund the contract even if they interact with it directly from a basic wallet instead of the ABI.

---

## Key Takeaways & Architecture

### 1. Vault vs. Ledger Separation
During development, I learned that a smart contract's internal bookkeeping ledger (`mapping` and `array` data) is entirely separate from its actual network vault balance (`address(this).balance`). 

### 2. The CEI Security Standard
To safeguard the withdrawal mechanism against malicious reentrancy attacks, the order of execution is rigorously maintained:
1. **Checks:** Verifying the user calling the function is the contract owner.
2. **Effects:** Zeroing out the ledger tracking mappings and clearing out the funder array first.
3. **Interactions:** Executing the `.call{value: balance}("")` low-level function to transfer funds safely out of the contract last.

---

## Contract Mechanics

### Deployment Specifications
- **Language:** Solidity `^0.8.19`
- **Target Network:** Ethereum Sepolia Testnet
- **Chainlink Data Feed Address (ETH/USD):** `0x694AA1769357215DE4FAC081bf1f309aDC325306`

### Core Functions
- `funds()`: Payable function allowing public users to transfer test ETH.
- `need1EthPrice()`: View function returning the 18-decimal scaled price of a single Ether.
- `getConversionPrice()`: Internal helper determining the total USD value of incoming transactions.
- `withdrawFunds()`: Restricted administrative function designed to reset internal state arrays and clear out the active contract vault pool.

---

## 🎓 Acknowledgments
This contract was independently rebuilt and optimized following architectural frameworks learned through the **Cyfrin Updraft Smart Contract Development Course**.
