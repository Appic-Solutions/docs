# EVM Minter

The **evm-minter** is the core component that enables token transfers between EVM chains and the Internet Computer Protocol (ICP). It provides secure, decentralized bridging capabilities without requiring off-chain dependencies.

## Overview

The evm-minter enables bidirectional token transfers by:

1. **Locking tokens** on the EVM chain
2. **Minting wrapped tokens** on the ICP chain
3. **Burning wrapped tokens** on ICP for withdrawals
4. **Unlocking original tokens** back to EVM chains

## Key Features

- **Secure ECDSA signing** using threshold cryptography
- **Automatic refunds** for failed transactions  
- **Multi-chain support** across major EVM networks
- **Native and ERC20 token** compatibility
- **No off-chain dependencies** for maximum security

## Security Model

The minter uses:
- **EVM RPC canister** for reliable blockchain communication
- **Threshold ECDSA signing** for transaction validation
- **Automated event collection** for deposit tracking
- **Time-locked refund mechanisms** for failed operations

## Supported Chains

| Chain | Network | Status |
|-------|---------|--------|
| Binance Smart Chain | BSC | ✅ Active |
| Ethereum | ETH | 🔄 Coming Soon |
| Polygon | MATIC | 🔄 Coming Soon |

## Next Steps

- [Deposit Flow](deposit.md) - Learn how to deposit tokens from EVM to ICP
- [Withdrawal Flow](withdrawal.md) - Learn how to withdraw tokens from ICP to EVM