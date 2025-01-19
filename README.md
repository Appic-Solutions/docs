# 🛠️ Introduction

Welcome to the **Appic** documentation! Appic is a bridging solution that connects Ethereum Virtual Machine (EVM) chains and the Internet Computer (ICP). This guide explains Appic's components and how they work together to enable secure and efficient cross-chain asset transfers.

### 📘 Prerequisites

Before diving in, you should know:

- Basics of the **Internet Computer (ICP)**.
- How **EVM chains** work and their functionality.

### 🌟 Overview

Appic bridges **EVM chains** and **ICP** with three main canisters:

1. [**evm-minter**](https://github.com/Appic-Solutions/evm-minter) (GitHub repository)
2. [**ledger-suite-manager (lsm)**](https://github.com/Appic-Solutions/ledger-suite-manager) (GitHub repository)
3. [**chain-fusion-helper**](https://github.com/Appic-Solutions/chain-fusion-helper) (GitHub repository)

These canisters work together to ensure secure and decentralized cross-chain transfers.

![Appic bridging overview](./images/overal-diagram.png "Appic bridging overview")

---

## 🔒 evm-minter

The **evm-minter** enables token transfers by:

1. **Locking tokens** on the EVM chain.
2. **Minting wrapped tokens** on the ICP chain.

### ✨ Use Cases

#### 1. Deposit (Lock Tokens)

- Convert native tokens (e.g., BNB) into wrapped tokens (e.g., icBNB).
- Convert ERC20 tokens (e.g., USDT) into wrapped ERC20 tokens (e.g., icUSDT).

#### 2. Withdrawal (Unlock Tokens)

- Convert wrapped tokens (e.g., icBNB) back to native tokens (e.g., BNB).
- Convert wrapped ERC20 tokens (e.g., icUSDT) back to ERC20 tokens (e.g., USDT).

### 🔐 Security

The **evm-minter** ensures security with:

- **EVM RPC canister** for reliable communication.
- **Threshold ECDSA signing** for secure validation.

No off-chain dependencies are required, ensuring safety and efficiency. Refunds are automatically processed if transactions fail.

### 📄 Integration Guide

- [Deposit Flow](./minter/deposit.md)
- [Withdrawal Flow](./minter/withdrawal.md)

---

## 📋 ledger-suite-manager (lsm)

The **lsm** canister handles:

1. **Creating twin tokens** on ICP for EVM tokens.
2. **Managing Ledger Suites** (Ledger, Index, Archive).
3. **Upgrading Ledger Suites** when needed.

### 🔧 Twin Token Creation

Developers can create wrapped ICP tokens for EVM assets supported by the **evm-minter**. Fees are paid in **ICP** or **Appic tokens**, and the process is fully automated.

### 📄 Integration Guide

- [Twin Token Creation](./lsm/create_twins.md)

---

## 🔍 chain-fusion-helper

The **chain-fusion-helper** canister provides essential data for bridging operations.

### 🔑 Key Features

- **Transaction Management**: Collects and organizes deposit and withdrawal data.
- **Query Endpoints**: Retrieve bridge transaction history using wallet addresses or ICP IDs.
- **Token Data**:
  - Lists over 1,000 EVM tokens across major chains.
  - Provides real-time ICP token prices.
  - Shows bridgeable tokens between ICP and EVM.
  - Tracks wrapped tokens created via the **lsm**.

The **chain-fusion-helper** centralizes this information for easy access.

### 📄 Integration Guide

- [Get Chain Fusion Data](./chain-fusion-helper/chain_fusion_data.md)
