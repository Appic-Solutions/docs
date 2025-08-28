## 🚀 Appic DEX Integration Guide

**Appic DEX** is a decentralized exchange built on the Internet Computer Protocol (ICP), providing trustless, permissionless trading with concentrated liquidity features. The DEX enables token swapping, liquidity management, and fee collection with enterprise-grade security and performance.

### 🛠️ Service Details

| Component | Information |
|-----------|------------|
| **Canister ID (Mainnet)** | [nbepk-iyaaa-aaaad-qhlma-cai](https://dashboard.internetcomputer.org/canister/nbepk-iyaaa-aaaad-qhlma-cai) |
| **Technology Stack** | Rust + Internet Computer |
| **Architecture** | Automated Market Maker (AMM) with Concentrated Liquidity |
| **License** | Apache-2.0 |

---

## ✨ Core Features

### **AMM Capabilities**
- **Concentrated Liquidity**: Capital concentration within custom price ranges
- **Multiple Fee Tiers**: Support for 0.01%, 0.05%, 0.1%, 0.3%, and 1.0% fee structures
- **Multi-hop Routing**: Complex swaps across multiple pools for optimal pricing
- **Price Oracle**: Tick-based pricing mechanism

### **Trading Features**
- **Exact Input/Output Swaps**: Control over swap amounts and slippage
- **Single & Multi-hop Swaps**: Direct trades or routed through intermediate tokens
- **Advanced Quoting**: Pre-execution price estimation for all swap types
- **Slippage Protection**: Configurable minimum output guarantees

### **Liquidity Management**
- **Position Creation**: Mint new liquidity positions within specified price ranges
- **Position Modifications**: Increase, decrease, or burn existing positions
- **Fee Collection**: Automated fee accrual with manual collection capabilities
- **Real-time Analytics**: Position performance tracking and historical data

### **Security & Reliability**
- **Principal Guards**: Prevents concurrent operations and double-spending attacks
- **Memory Safety**: Rust-based implementation with comprehensive error handling
- **ICRC Integration**: Secure token interactions using Internet Computer standards
- **Event Logging**: Complete audit trail for all operations

---

## 📋 Supported Operations

| Category | Operations | Description |
|----------|------------|-------------|
| **Pool Management** | create_pool | Initialize new trading pools |
| **Trading** | swap, quote | Execute trades and price estimation |
| **Liquidity** | mint_position, increase_liquidity, decrease_liquidity, burn | Position lifecycle management |
| **Fee Collection** | collect_fees | Harvest trading fees from positions |
| **Queries** | get_pool, get_pools, get_position, user_balance, get_events | Data retrieval and analytics |

---

## 🎯 Integration Architecture

### **Pool Structure**

```rust
type CandidPoolId = record {
  fee: nat;
  token0: principal;
  token1: principal;
}
```

### **Position Management**

```rust
type CandidPositionKey = record {
  owner: principal;
  pool: CandidPoolId;
  tick_lower: int;
  tick_upper: int;
}
```

### **Tick System**

Appic DEX uses a tick-based pricing system where each tick represents a 0.01% price change:

```
Price = 1.0001^tick
```

**Common Tick Ranges:**
- **Wide Range**: -887220 to 887220 (full range)
- **Narrow Range**: -1000 to 1000 (±10% from current price)
- **Tight Range**: -200 to 200 (±2% from current price)

---

## 📡 API Reference

### 🏊 Pool Management

#### Create Pool
Creates a new liquidity pool with specified parameters.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai create_pool '(record { 
  fee = 3000 : nat; 
  sqrt_price_x96 = 79228162514264337593543950336 : nat; 
  token_a = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
  token_b = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
})'
```

**Parameters:**
- `fee`: Fee tier in basis points (100, 500, 1000, 3000, 10000)
- `sqrt_price_x96`: Initial price in sqrt(price) * 2^96 format
- `token_a`: First token principal
- `token_b`: Second token principal

**Response:**
```candid
variant { Ok = record { fee = 3000 : nat; token0 = principal "..."; token1 = principal "..." } }
```

**Common Errors:**
- `InvalidSqrtPriceX96`: Price outside valid range
- `InvalidFeeAmount`: Unsupported fee tier
- `PoolAlreadyExists`: Pool with same parameters exists
- `DuplicatedTokens`: Same token used for both positions

### 💱 Trading Operations

#### Execute Swap (Exact Input Single)
Swap exact amount of input token for minimum output.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai swap '(variant { 
  ExactInputSingle = record { 
    zero_for_one = true; 
    from_subaccount = null; 
    amount_out_minimum = 950000000 : nat; 
    amount_in = 1000000000 : nat; 
    pool_id = record { 
      fee = 3000 : nat; 
      token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
      token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
    } 
  } 
})'
```

**Parameters:**
- `zero_for_one`: Swap direction (token0 → token1 or token1 → token0)
- `amount_in`: Exact input amount
- `amount_out_minimum`: Minimum acceptable output
- `pool_id`: Target pool identifier
- `from_subaccount`: Source subaccount (optional)

**Response:**
```candid
variant { Ok = record { amount_out = 975234567 : nat; amount_in = 1000000000 : nat } }
```

#### Execute Swap (Exact Output Single)
Swap maximum input for exact output amount.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai swap '(variant { 
  ExactOutputSingle = record { 
    zero_for_one = true; 
    from_subaccount = null; 
    amount_in_maximum = 1050000000 : nat; 
    amount_out = 1000000000 : nat; 
    pool_id = record { 
      fee = 3000 : nat; 
      token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
      token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
    } 
  } 
})'
```

#### Multi-hop Swap (Exact Input)
Execute multi-hop swaps across multiple pools.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai swap '(variant { 
  ExactInput = record { 
    amount_out_minimum = 45000000 : nat; 
    amount_in = 1000000000 : nat; 
    recipient = principal "your-principal-here"; 
    path = vec { 
      principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
      record { fee = 3000 : nat; intermediary_token = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" }; 
      principal "mxzaz-hqaaa-aaaar-qaada-cai" 
    }; 
    from_subaccount = null 
  } 
})'
```

#### Get Quote
Estimate swap output without executing the trade.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai quote '(variant { 
  QuoteExactInputSingleParams = record { 
    zero_for_one = true; 
    pool_id = record { 
      fee = 3000 : nat; 
      token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
      token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
    }; 
    exact_amount = 1000000000 : nat 
  } 
})'
```

**Quote Response:**
```candid
variant { Ok = 975234567 : nat }
```

### 🏦 Liquidity Management

#### Mint New Position
Create a new liquidity position within specified price range.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai mint_position '(record { 
  amount1_max = 1000000000 : nat; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  from_subaccount = null; 
  amount0_max = 1000000000 : nat; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

**Parameters:**
- `amount0_max`: Maximum token0 to deposit
- `amount1_max`: Maximum token1 to deposit
- `tick_lower`: Lower price boundary
- `tick_upper`: Upper price boundary

**Response:**
```candid
variant { Ok = 12345 : nat }
```

#### Increase Liquidity
Add more liquidity to existing position.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai increase_liquidity '(record { 
  amount1_max = 500000000 : nat; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  from_subaccount = null; 
  amount0_max = 500000000 : nat; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

#### Decrease Liquidity
Remove portion of liquidity from position.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai decrease_liquidity '(record { 
  amount1_min = 200000000 : nat; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  liquidity = 50000000000 : nat; 
  amount0_min = 200000000 : nat; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

**Parameters:**
- `liquidity`: Amount of liquidity to remove
- `amount0_min`: Minimum token0 to receive
- `amount1_min`: Minimum token1 to receive

#### Collect Fees
Harvest accumulated trading fees from position.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai collect_fees '(record { 
  owner = principal "your-principal-here"; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

**Response:**
```candid
variant { 
  Ok = record { 
    amount0_collected = 1234567 : nat; 
    amount1_collected = 987654 : nat 
  } 
}
```

#### Burn Position
Permanently remove entire liquidity position.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai burn '(record { 
  amount1_min = 450000000 : nat; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  amount0_min = 450000000 : nat; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

### 📊 Data & Analytics

#### Get Pool State
Retrieve current pool information and statistics.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_pool '(record { 
  fee = 3000 : nat; 
  token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
  token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
})'
```

**Response Example:**
```candid
opt record {
  sqrt_price_x96 = 79228162514264337593543950336 : nat;
  tick = 0 : int;
  fee = 3000 : nat;
  token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai";
  token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai";
  liquidity = 1000000000000000000 : nat;
  fee_growth_global0_x128 = 0 : nat;
  fee_growth_global1_x128 = 0 : nat;
}
```

#### List All Pools
Get overview of all available pools.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_pools '()'
```

#### Get Position Details
Retrieve specific position information and performance.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_position '(record { 
  owner = principal "your-principal-here"; 
  pool = record { 
    fee = 3000 : nat; 
    token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
    token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
  }; 
  tick_lower = -1000 : int; 
  tick_upper = 1000 : int 
})'
```

**Response Example:**
```candid
opt record {
  liquidity = 50000000000 : nat;
  fee_growth_inside0_last_x128 = 12345 : nat;
  fee_growth_inside1_last_x128 = 67890 : nat;
  tokens_owed0 = 123456 : nat;
  tokens_owed1 = 654321 : nat;
}
```

#### Get User Positions
List all positions owned by a specific user.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_positions_by_owner '(principal "your-principal-here")'
```

#### Check User Balance
Get user's balance for specific token within DEX.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai user_balance '(record { 
  token = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
  user = principal "your-principal-here" 
})'
```

#### Get All User Balances
Retrieve all token balances for a user.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai user_balances '(principal "your-principal-here")'
```

#### Get Historical Data
Access pool performance and trading history.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_pool_history '(record { 
  fee = 3000 : nat; 
  token0 = principal "rdmx6-jaaaa-aaaah-qcaiq-cai"; 
  token1 = principal "ryjl3-tyaaa-aaaaa-aaaba-cai" 
})'
```

#### Get Events
Retrieve transaction history and events.

```bash
dfx canister call nbepk-iyaaa-aaaad-qhlma-cai get_events '(record { 
  start = 0 : nat64; 
  length = 100 : nat64 
})'
```

**Event Types:**
- `Swap`: Token exchange transactions
- `CreatedPool`: New pool creation
- `MintedPosition`: New liquidity positions
- `IncreasedLiquidity`: Position size increases
- `DecreasedLiquidity`: Position size decreases
- `BurntPosition`: Position removals
- `CollectedFees`: Fee harvesting

---

## 🛡️ Error Handling

### Common Error Types

#### Pool Errors
```candid
CreatePoolError = variant {
  InvalidSqrtPriceX96;
  InvalidFeeAmount;
  DuplicatedTokens;
  InvalidToken : principal;
  PoolAlreadyExists;
}
```

#### Swap Errors
```candid
SwapError = variant {
  InvalidAmountOut;
  InvalidAmountIn;
  DepositError : DepositError;
  PoolNotInitialized;
  LockedPrincipal;
  NoInRangeLiquidity;
  SwapFailedRefunded;
}
```

#### Position Errors
```candid
MintPositionError = variant {
  DepositError : DepositError;
  TickNotAlignedWithTickSpacing;
  InvalidAmount;
  InvalidPoolFee;
  PoolNotInitialized;
  InsufficientBalance;
  LiquidityOverflow;
  PositionAlreadyExists;
  InvalidTick;
  LockedPrincipal;
  SlippageFailed;
}
```

### Error Handling Best Practices

1. **Always Check Return Values**: All operations return Results with detailed error information
2. **Handle Slippage**: Set appropriate minimum/maximum amounts for price protection
3. **Verify Token Approvals**: Ensure sufficient ICRC2 allowances before operations
4. **Monitor Principal Locks**: Avoid concurrent operations from same principal
5. **Validate Tick Ranges**: Ensure ticks are within valid bounds and properly aligned

---

## 🔧 Integration Patterns

### **Price Calculation Utilities**

```javascript
function tickToPrice(tick) {
  return Math.pow(1.0001, tick);
}

function priceToTick(price) {
  return Math.floor(Math.log(price) / Math.log(1.0001));
}

function tickToSqrtPriceX96(tick) {
  const price = tickToPrice(tick);
  const sqrtPrice = Math.sqrt(price);
  const Q96 = BigInt(2) ** BigInt(96);
  return BigInt(Math.floor(sqrtPrice * Number(Q96)));
}
```

### **Position Management Workflow**

```javascript
class LiquidityManager {
  async createPosition(poolId, tickLower, tickUpper, amount0, amount1) {
    try {
      await this.approveToken(poolId.token0, amount0);
      await this.approveToken(poolId.token1, amount1);
      
      const result = await this.dexCanister.mint_position({
        pool: poolId,
        tick_lower: tickLower,
        tick_upper: tickUpper,
        amount0_max: amount0,
        amount1_max: amount1,
        from_subaccount: null
      });
      
      if (result.Ok) {
        return result.Ok;
      } else {
        throw new Error(`Position creation failed: ${result.Err}`);
      }
    } catch (error) {
      console.error('Position creation error:', error);
      throw error;
    }
  }
  
  async collectFees(positionKey) {
    const result = await this.dexCanister.collect_fees(positionKey);
    if (result.Ok) {
      return {
        amount0: result.Ok.amount0_collected,
        amount1: result.Ok.amount1_collected
      };
    } else {
      throw new Error(`Fee collection failed: ${result.Err}`);
    }
  }
}
```

### **Swap Execution Patterns**

```javascript
class SwapManager {
  async exactInputSwap(poolId, amountIn, minAmountOut, zeroForOne) {
    try {
      const swapArgs = {
        ExactInputSingle: {
          pool_id: poolId,
          zero_for_one: zeroForOne,
          amount_in: amountIn,
          amount_out_minimum: minAmountOut,
          from_subaccount: null
        }
      };
      
      const result = await this.dexCanister.swap(swapArgs);
      
      if (result.Ok) {
        return {
          amountIn: result.Ok.amount_in,
          amountOut: result.Ok.amount_out
        };
      } else {
        throw new Error(`Swap failed: ${result.Err}`);
      }
    } catch (error) {
      console.error('Swap error:', error);
      throw error;
    }
  }
  
  async getQuote(poolId, amountIn, zeroForOne) {
    const quoteArgs = {
      QuoteExactInputSingleParams: {
        pool_id: poolId,
        exact_amount: amountIn,
        zero_for_one: zeroForOne
      }
    };
    
    const result = await this.dexCanister.quote(quoteArgs);
    return result.Ok || null;
  }
}
```

---

## 📈 Performance Optimization

### **Batch Operations**
- Group multiple queries into single calls when possible
- Use `get_pools()` instead of multiple `get_pool()` calls
- Combine balance checks with position queries

### **Efficient Price Ranges**
- **Narrow Ranges**: Higher capital efficiency but more rebalancing
- **Wide Ranges**: Lower maintenance but reduced fee capture
- **Active Management**: Regular position adjustment for optimal performance

### **Gas Optimization**
- Approve maximum amounts to reduce approval transactions
- Use exact output swaps when final amount is critical
- Batch fee collection with position management

---

## 🔒 Security Best Practices

### **Token Approvals**
```bash
dfx canister call <token_canister> icrc2_approve '(record {
  amount = 1000000000 : nat;
  spender = record {
    owner = principal "nbepk-iyaaa-aaaad-qhlma-cai";
    subaccount = null;
  };
})'
```

### **Slippage Protection**
- **Conservative**: 0.1-0.5% slippage tolerance
- **Standard**: 0.5-1.0% slippage tolerance
- **Aggressive**: 1.0-3.0% slippage tolerance (volatile markets)

### **Principal Management**
- Use unique principals for different operations
- Avoid concurrent operations from same principal
- Monitor for `LockedPrincipal` errors

### **Position Security**
- Regularly collect fees to avoid overflow
- Monitor position health and impermanent loss
- Set appropriate tick ranges for risk tolerance

---

## 🚀 Advanced Use Cases

### **Arbitrage Trading**
1. Monitor price differences across pools
2. Execute atomic multi-hop swaps
3. Use exact output swaps for precise arbitrage

### **Liquidity Mining**
1. Create positions in high-volume pools
2. Implement automatic rebalancing strategies
3. Compound collected fees into new positions

### **Portfolio Rebalancing**
1. Use multi-hop swaps for token conversions
2. Adjust position ranges based on market conditions
3. Implement stop-loss mechanisms with position burning

### **Market Making**
1. Create tight-range positions around current price
2. Implement dynamic fee collection strategies
3. Use historical data for optimal range selection

---

## 📞 Support & Resources

### **Documentation**
- [Pool Management Guide](https://github.com/Appic-Solutions/appic-dex/blob/main/pool.md)
- [Trading Guide](https://github.com/Appic-Solutions/appic-dex/blob/main/swap.md)
- [Query Reference](https://github.com/Appic-Solutions/appic-dex/blob/main/queries.md)
- [Architecture Overview](https://github.com/Appic-Solutions/appic-dex/blob/main/architecture.md)

### **Development Resources**
- **GitHub Repository**: https://github.com/Appic-Solutions/appic-dex
- **Candid Interface**: Available in repository
- **IC Dashboard**: https://dashboard.internetcomputer.org/canister/nbepk-iyaaa-aaaad-qhlma-cai

### **Support Channels**
- **Email**: tech@appicdao.com
- **GitHub Issues**: Bug reports and feature requests
- **ICP Community**: Internet Computer developer forums

---

## 🎯 Quick Start Checklist

### **Prerequisites**
- [ ] DFX SDK installed (version 0.20.0+)
- [ ] Rust development environment
- [ ] ICRC-compatible tokens
- [ ] Principal identity configured

### **Basic Integration**
- [ ] Deploy or connect to DEX canister
- [ ] Approve tokens for DEX spending
- [ ] Create or join existing pools
- [ ] Execute test swaps with small amounts
- [ ] Monitor positions and collect fees

### **Production Deployment**
- [ ] Implement comprehensive error handling
- [ ] Set up monitoring and alerting
- [ ] Configure appropriate slippage tolerances
- [ ] Test all edge cases and failure scenarios
- [ ] Implement security best practices

---

## ⚡ Key Features Summary

- ✅ **Concentrated Liquidity** - Capital efficient liquidity provision
- ✅ **Multiple Fee Tiers** - 0.01% to 1.0% fee options
- ✅ **Multi-hop Routing** - Complex swap paths supported
- ✅ **Advanced Quoting** - Price estimation system
- ✅ **Real-time Analytics** - Comprehensive data access
- ✅ **Security First** - Rust-based with extensive testing
- ✅ **ICRC Integration** - Native IC token standard support
- ✅ **Event Logging** - Complete audit trail
- ✅ **Principal Guards** - Concurrent operation protection
- ✅ **Slippage Protection** - Configurable price tolerance

The Appic DEX provides a robust foundation for decentralized trading on the Internet Computer, combining the efficiency of concentrated liquidity with the security and scalability of ICP.