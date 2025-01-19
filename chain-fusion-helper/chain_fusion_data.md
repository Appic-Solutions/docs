## 🔍 Chain-fusion-helper Integration Guide

This guide explains how to use **chain-fusion-helper** to retrieve bridge transaction data, live USD prices for ICP tokens, EVM token information, and bridgeable tokens available on ICP.

### 🛠️ Canister Details

| Name                | Principal                   |
| ------------------- | --------------------------- |
| chain-fusion-helper | zjydy-zyaaa-aaaaj-qnfka-cai |

### 🔄 Transaction Data

The **chain-fusion-helper** canister automatically collects transactions between ICP and EVM chains through Appic or ckETH minters (provided by DFINITY). These transactions are updated every minute.

#### 📂 Query Endpoints

1. **Get a Transaction by Parameters**

   ```candid
   get_transaction : (GetTxParams) -> (opt Transaction) query;
   ```

   - **Parameters**:

     ```candid
     type GetTxParams = record {
         chain_id : nat;
         search_param : TransactionSearchParam;
     };

     type TransactionSearchParam = variant {
         TxWithdrawalId : nat;
         TxMintId : nat;
         TxHash : text;
     };
     ```

   - **Returned Format**:
     ```candid
     type Transaction = variant {
         EvmToIcp : CandidEvmToIcp;
         IcpToEvm : CandidIcpToEvm;
     };
     ```

2. **Get Transactions by Address**

   ```candid
   get_txs_by_address : (text) -> (vec Transaction) query;
   ```

   Retrieves all transactions for a specified EVM wallet address.

3. **Get Transactions by Address and Principal**

   ```candid
   get_txs_by_address_principal_combination : (text, principal) -> (vec Transaction) query;
   ```

   Retrieves all transactions for a combination of an EVM wallet address and an ICP principal ID.

4. **Get Transactions by Principal**
   ```candid
   get_txs_by_principal : (principal) -> (vec Transaction) query;
   ```
   Retrieves all transactions for a specified ICP principal ID.

#### 🔀 Transaction Variants

Transactions can be of two types:

- **EvmToIcp**: Deposits from EVM to ICP (e.g., BNB ➔ icBNB).
- **IcpToEvm**: Withdrawals from ICP to EVM (e.g., icUSDT ➔ USDT).

Each transaction includes details such as status, value, time, and addresses.

---

### 🪙 Tokens List

The **chain-fusion-helper** canister stores lists of:

1. **ICP tokens**: Metadata includes name, symbol, decimals, logo, ledger ID, and live USD prices.
2. **EVM tokens**: Metadata includes name, symbol, decimals, logo, chain ID, and contract address.
3. **Bridgeable tokens**: Token pairs with one token on ICP and one on EVM.

#### 📂 Query Endpoints for Tokens

1. **Get Bridgeable Token Pairs**

   ```candid
   get_bridge_pairs : () -> (vec TokenPair) query;
   ```

   Retrieves a list of bridgeable token pairs.

2. **Get EVM Token by Parameters**

   ```candid
   get_evm_token : (GetEvmTokenArgs) -> (opt CandidEvmToken) query;
   ```

   Parameters:

   ```candid
   type GetEvmTokenArgs = record { chain_id : nat; address : text };
   ```

3. **Get ICP Token by Parameters**

   ```candid
   get_icp_token : (GetIcpTokenArgs) -> (opt CandidIcpToken) query;
   ```

   Parameters:

   ```candid
   type GetIcpTokenArgs = record { ledger_id : principal };
   ```

4. **Get All ICP Tokens**
   ```candid
   get_icp_tokens : () -> (vec CandidIcpToken) query;
   ```
   Retrieves a list of all ICP tokens with live USD prices.

#### 📝 Token Metadata Formats

1. **EVM Token Metadata**

   ```candid
   type CandidEvmToken = record {
       decimals : nat8;
       logo : text;
       name : text;
       erc20_contract_address : text;
       chain_id : nat;
       symbol : text;
   };
   ```

2. **ICP Token Metadata**

   ```candid
   type CandidIcpToken = record {
       fee : nat;
       decimals : nat8;
       usd_price : text;
       logo : text;
       name : text;
       rank : opt nat32;
       ledger_id : principal;
       token_type : IcpTokenType;
       symbol : text;
   };
   ```

3. **TokenPair Format**
   ```candid
   type TokenPair = record {
       operator : Operator;
       evm_token : CandidEvmToken;
       icp_token : CandidIcpToken;
   };
   ```

For more information visit [chain-fusion-github-helper github](https://github.com/Appic-Solutions/chain-fusion-helper/)
