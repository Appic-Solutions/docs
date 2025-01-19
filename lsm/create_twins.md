## 🌟 Ledger-Suite-Manager (LSM) Integration Guide

This guide explains how to use **Ledger-Suite-Manager (LSM)** to create twin tokens on ICP for an EVM token efficiently and securely.

### 🛠️ Canister Details

| **Name**             | **Principal**               |
| -------------------- | --------------------------- |
| Ledger-Suite-Manager | kmcdp-4yaaa-aaaag-ats3q-cai |

### 🔄 Twin Token Creation

The LSM canister supports:

- **Twin Token Creation**
- **Ledger Suite Maintenance**
- **Upgrades**

This guide focuses on creating a twin token.

#### 💰 Cost of Twin Token Creation

Creating a twin token costs **25 ICP tokens**. You can fetch the latest cost using the `get_lsm_info` endpoint.

1. **Set Allowance**:
   Call the `icrc2_approve` function from the ICP ledger to set an allowance equal to the required fee.

   **Example (25 ICP):**

   ```bash
   dfx canister call ryjl3-tyaaa-aaaaa-aaaba-cai icrc2_approve \
   "(record { amount = 2_500_000_000; spender = record { owner = principal \"kmcdp-4yaaa-aaaag-ats3q-cai\";} })" --network ic
   ```

2. **Call `add_erc20_ls`**:
   After approval, call the `add_erc20_ls` endpoint. The fee is deducted automatically, and the twin token is created using your provided parameters.

#### 📝 Parameters for Twin Token Creation

- Ensure `LedgerInitArg` matches the ERC20 token details.
- The `token_logo` must be in **Data URI** format.

**Example Parameters:**
Creating **icUSDT.pol** for **USDT** on **Polygon**:

```candid
type AddErc20Arg = record {
  contract : Erc20Contract {
    record {
      chain_id : 137; // Polygon chain ID
      address : "0xc2132d05d31c914a87c6611c10748aeb04b58e8f"; // USDT address
    }
  };
  ledger_init_arg : record {
    decimals : 6; // USDT has 6 decimals
    token_symbol : "icUSDT.pol"; // Represents USDT on Polygon
    transfer_fee : 10000; // 0.01 USDT after applying decimals
    token_logo : "data:image/png;base64,iVBORw0KGgoAAA......."; // Logo in Data URI format
    token_name : "USDT on ICP";
  };
};
```

The twin token creation process typically takes **2 to 10 minutes**.

### 🔍 Checking Twin Token Status

To check your twin token's status, call:

```candid
all_twins_canister_ids : () -> (vec ManagedCanisters) query;
```

#### Status Indicators:

- **Created**: The process is ongoing.
- **Installed**: The twin token has been successfully created.

### 🚨 Handling Errors

If the `add_erc20_ls` endpoint returns an error, the issues could include:

- Invalid parameters.
- Twin token already exists.
- Insufficient ICP balance.

#### Common Errors

```candid
type AddErc20Error = variant {
  TransferIcpError : TransferFromError;
  ChainIdNotSupported : text;
  Erc20TwinTokenAlreadyExists;
  InvalidErc20Contract : text;
  InternalError : text;
};
```

### 🔧 Key Endpoints

1. **Fetch LSM Info:**

   ```candid
   get_lsm_info : () -> (LedgerManagerInfo) query;
   ```

   Retrieves the latest LSM details, including costs and managed canisters.

2. **Check Twin Token Status:**

   ```candid
   all_twins_canister_ids : () -> (vec ManagedCanisters) query;
   ```

   Returns all twin tokens and their statuses.

3. **Create Twin Token:**
   ```candid
   add_erc20_ls : (AddErc20Arg) -> (Result);
   ```
   Initiates the twin token creation process.

For more details, visit the [Ledger-Suite-Manager GitHub Repository](https://github.com/Appic-Solutions/ledger-suite-manager/).
