## 🚀 Smart DEX Aggregator Integration Guide

The **Smart DEX Aggregator** is an enterprise-grade DEX aggregator providing optimal token swap routes across multiple blockchain networks with Internet Computer Protocol (ICP) integration and cross-chain capabilities.

### 🛠️ Service Details

| Name | URL |
|------|-----|
| Smart Routing API | https://quoter.appicdao.com |
| API Documentation | https://quoter.appicdao.com/api-docs |

### ✨ Core Features

- **Multi-Protocol Aggregation**: Uniswap V2/V3/V4, PancakeSwap, SushiSwap integration
- **Cross-Chain Routing**: Seamless token swaps across different blockchains via ICP bridge
- **Internet Computer Integration**: Zero gas fees DEX routes with ICP protocol
- **Smart Route Optimization**: Advanced algorithms for best price discovery
- **Real-time Gas Estimation**: Dynamic gas calculation with USD pricing
- **Comprehensive Slippage Protection**: Multi-step slippage calculation for cross-chain routes
- **Adaptive RPC Management**: Automatic failover and load balancing across RPC providers
- **Intelligent Caching**: Multi-layer caching for pools, quotes, and token data
- **Performance Monitoring**: Built-in metrics and health checks
- **Error Resilience**: Robust error handling with timeout management

---

## 📋 Supported Networks

| Network | Chain ID | Native Token | Protocols | Status |
|---------|----------|--------------|-----------|---------|
| **Ethereum** | 1 | ETH | Uniswap V2/V3/V4 | ✅ Active |
| **Base** | 8453 | ETH | Uniswap V3 | ✅ Active |
| **BSC** | 56 | BNB | PancakeSwap V2/V3 | ✅ Active |
| **Arbitrum** | 42161 | ETH | Uniswap V3 | ✅ Active |
| **Internet Computer** | ICP | ICP | ICP DEX | ✅ Active |

### 🔗 Cross-Chain Support Matrix

```
┌─────────────┬─────────────┬─────────────┬─────────────┬─────────────┐
│             │  Ethereum   │    Base     │     BSC     │   Arbitrum  │
├─────────────┼─────────────┼─────────────┼─────────────┼─────────────┤
│  Ethereum   │      -      │      ✅     │      ✅     │      ✅     │
│    Base     │      ✅     │      -      │      ✅     │      ✅     │
│     BSC     │      ✅     │      ✅     │      -      │      ✅     │
│   Arbitrum  │      ✅     │      ✅     │      ✅     │      -      │
└─────────────┴─────────────┴─────────────┴─────────────┴─────────────┘
```

---

## 🎯 Quote Types & Capabilities

### 📊 Single-Chain Quotes

#### **Standard ERC-20 Token Swaps**
- **Direct Routes**: Token A ↔ Token B via single DEX pool
- **Multi-hop Routes**: Token A → Intermediate Token → Token B for optimal pricing
- **Protocol Comparison**: Automatically compares Uniswap V2/V3/V4, PancakeSwap, SushiSwap

```bash
# Example: USDC → WETH on Ethereum
curl "https://quoter.appicdao.com/api/quote?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000&chainId=1" | jq
```

#### **Native Token Support**
- **ETH/BNB/MATIC Integration**: Seamless native token handling with automatic wrapping
- **Wrap/Unwrap Operations**: Automatic WETH wrapping for native ETH transactions
- **Gas Optimization**: Enhanced gas calculations for wrap/unwrap operations

```bash
# Example: ETH → USDC on Base (Native ETH input)
curl "https://quoter.appicdao.com/api/quote?tokenIn=0x0000000000000000000000000000000000000000&tokenOut=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&amountIn=100000000000000000&chainId=8453" | jq

# Example: USDC → ETH on Base (Native ETH output)  
curl "https://quoter.appicdao.com/api/quote?tokenIn=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&tokenOut=0x0000000000000000000000000000000000000000&amountIn=100000000&chainId=8453" | jq
```

### 🌉 Cross-Chain Quotes

#### **EVM-to-EVM Cross-Chain Routing**
- **Multi-Step Processing**: TokenA (ChainA) → USDC (ChainA) → ckUSDC (ICP) → USDC (ChainB) → TokenB (ChainB)
- **Intelligent Bridge Selection**: Automatic routing via ICP bridge infrastructure
- **Gas-Aware Routing**: Considers gas costs across all chains in route optimization

```bash
# Example: USDC on BSC → USDC on Base
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000" | jq

# Example: BNB → USDC Cross-Chain (Native token cross-chain)
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x0000000000000000000000000000000000000000&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000000000000" | jq
```

#### **EVM-to-ICP Integration**
- **Direct ICP DEX Access**: Route directly to Internet Computer DEX for zero gas fees
- **Token Mapping**: Automatic mapping between EVM tokens and ICP token principals
- **Hybrid Routing**: Compare EVM and ICP routes for optimal execution

#### **Cross-Chain Native Token Support**
- **Native-to-ERC20**: BNB → USDC across chains with automatic wrapping
- **ERC20-to-Native**: USDC → ETH across chains with automatic unwrapping
- **Native-to-Native**: ETH → BNB cross-chain routing

```bash
# Example: USDC on BSC → ETH on Base (Cross-chain to native)
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x0000000000000000000000000000000000000000&chainB=8453&amount=50000000000000000000" | jq
```

### 🔗 Internet Computer Protocol (ICP) Quotes

#### **Pure ICP DEX Routing**
- **Zero Gas Fees**: Execute trades on ICP DEX with no gas costs
- **Principal-based Trading**: Direct trading using ICP token principals
- **Canister Integration**: Real-time quotes from ICP DEX canisters

```bash
# Example: Direct ICP quote
curl "https://quoter.appicdao.com/api/icp/quote?tokenIn=rdmx6-jaaaa-aaaah-qcaiq-cai&tokenOut=rrkah-fqaaa-aaaah-qcaiq-cai&amountIn=1000000" | jq
```

#### **ICP Bridge Integration**
- **ckUSDC Variants**: Support for ckUSDC.ETH, ckUSDC.BSC, ckUSDC.BASE
- **Multi-hop ICP Routing**: Automatic routing through ICP liquidity pools
- **Bridge Fee Calculation**: Transparent bridge fee estimation

### 🚀 Advanced Quote Features

#### **Batch Quote Processing**
- **Multiple Pairs**: Get quotes for multiple token pairs simultaneously
- **Portfolio Rebalancing**: Optimize multiple swaps in a single request
- **Route Comparison**: Compare different routing strategies

```bash
curl -X POST https://quoter.appicdao.com/api/quote/batch \
  -H "Content-Type: application/json" \
  -d '{
    "pairs": [
      {"tokenIn": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", "tokenOut": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "amountIn": "1000000"},
      {"tokenIn": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "tokenOut": "0x6B175474E89094C44Da98b954EedeAC495271d0F", "amountIn": "2000000"}
    ]
  }' | jq
```

#### **Route Comparison & Analysis**
- **EVM vs ICP**: Side-by-side comparison of routing options
- **Protocol Analysis**: Detailed breakdown of why specific routes were chosen
- **Gas Impact Analysis**: Show gas costs vs. price impact trade-offs

```bash
curl "https://quoter.appicdao.com/api/quote/compare?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000" | jq
```

---

## 📡 API Endpoints

### 🔍 Core Quote Endpoints

#### Get Best Quote
```http
GET /api/quote?tokenIn={address}&tokenOut={address}&amountIn={amount}&chainId={id}
```

**Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `tokenIn` | address | Yes | Input token contract address | `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48` |
| `tokenOut` | address | Yes | Output token contract address | `0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2` |
| `amountIn` | string | Yes | Amount in token's smallest unit | `1000000` |
| `chainId` | number | No | Target blockchain (default: 1) | `1`, `56`, `8453`, `42161` |
| `includeICP` | boolean | No | Include ICP routes | `true`, `false` |
| `slippageTolerance` | number | No | Slippage tolerance in basis points | `50` (0.5%) |

**Example Request:**
```bash
curl "https://quoter.appicdao.com/api/quote?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000&chainId=1" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "bestRoute": {
      "protocol": "uniswap_v3",
      "chain": "Ethereum",
      "tokenIn": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
      "tokenOut": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
      "amountIn": "1000000",
      "amountOut": "213767109419102",
      "executionPrice": "0.000214",
      "route": [
        {
          "protocol": "v3",
          "fee": "100",
          "sell_token": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
          "buy_token": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
          "pool_address": "0xE0554a476A092703abdB3Ef35c80e0D76d32939F"
        }
      ],
      "path": [
        "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"
      ],
      "score": 95,
      "gasLimit": "229870",
      "maxGasFee": "6473302101",
      "minAmountOut": "212698273872006",
      "slippage": "0.5%",
      "gasPriceUSD": "6.967357"
    },
    "summary": {
      "totalRoutes": 1,
      "evmRoutes": 1,
      "icpRoutes": 0,
      "bestChain": "EVM",
      "searchTime": "2025-08-13T14:48:40.825Z"
    }
  }
}
```

#### Route Comparison
```http
GET /api/quote/compare?tokenIn={address}&tokenOut={address}&amountIn={amount}&chainId={id}
```

**Example:**
```bash
curl "https://quoter.appicdao.com/api/quote/compare?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000" | jq
```

**Response Example:**
```json
{
  "success": true,
  "data": {
    "metrics": {
      "totalRoutes": 8,
      "bestOutput": "1855340000",
      "outputComparison": "+0.27% better on ICP",
      "gasComparison": "100% gas savings on ICP",
      "speedComparison": "80% faster on ICP",
      "recommendation": {
        "chain": "ICP",
        "reasoning": "Better price and zero gas fees"
      }
    },
    "routes": {
      "evm": {
        "count": 3,
        "best": {
          "protocol": "uniswap_v3",
          "amountOut": "1850250000",
          "score": 95.5
        }
      },
      "icp": {
        "count": 5,
        "best": {
          "protocol": "icp",
          "amountOut": "1855340000",
          "estimatedTime": "3000",
          "isDirect": true,
          "score": 97.2
        }
      }
    }
  }
}
```

#### Best Route Search
```http
GET /api/quote/best?tokenIn={address}&tokenOut={address}&amountIn={amount}
```

```bash
curl "https://quoter.appicdao.com/api/quote/best?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000" | jq
```

#### Protocol Status
```http
GET /api/quote/protocols
```

```bash
curl "https://quoter.appicdao.com/api/quote/protocols" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "totalProtocols": 4,
    "availableProtocols": ["uniswap_v2", "uniswap_v3", "uniswap_v4", "icp"],
    "protocolDetails": {
      "uniswap_v2": {
        "name": "Uniswap V2",
        "available": true,
        "chains": ["Ethereum", "Base", "BSC"],
        "type": "EVM"
      },
      "icp": {
        "name": "ICP DEX",
        "available": true,
        "chains": ["Internet Computer"],
        "type": "ICP",
        "features": [
          "Zero gas fees",
          "Multi-hop routing",
          "Fast execution",
          "Native IC integration"
        ]
      }
    },
    "icpStatus": {
      "enabled": true,
      "healthy": true,
      "supportedTokens": 15,
      "poolsLoaded": 42
    }
  }
}
```

#### Batch Quote Processing
```http
POST /api/quote/batch
```

**Request Example:**
```bash
curl -X POST https://quoter.appicdao.com/api/quote/batch \
  -H "Content-Type: application/json" \
  -d '{
    "pairs": [
      {"tokenIn": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", "tokenOut": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "amountIn": "1000000"},
      {"tokenIn": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2", "tokenOut": "0x6B175474E89094C44Da98b954EedeAC495271d0F", "amountIn": "2000000"}
    ],
    "chainId": "1",
    "includeICP": "true"
  }' | jq
```

### 🌉 Cross-Chain Endpoints

#### Cross-Chain Quote
```http
GET /api/quote/cross-chain?tokenA={address}&chainA={id}&tokenB={address}&chainB={id}&amount={amount}
```

**Example Requests:**

**Basic Cross-Chain USDC:**
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000" | jq
```

**Native Token Cross-Chain (BNB → USDC):**
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x0000000000000000000000000000000000000000&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000000000000" | jq
```

**Cross-Chain to Native (USDC → ETH):**
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x0000000000000000000000000000000000000000&chainB=8453&amount=50000000000000000000" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "route": {
      "source": {
        "chain": "BNB Smart Chain",
        "chainId": 56,
        "token": "0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d",
        "amount": "100000000"
      },
      "destination": {
        "chain": "Base",
        "chainId": 8453,
        "token": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
        "amount": "99500000"
      },
      "bridge": {
        "protocol": "ICP",
        "steps": ["BNB Smart Chain → ICP", "ICP → Base"]
      }
    },
    "pricing": {
      "executionPrice": "0.99500",
      "priceImpact": "0.25%",
      "minimumOutput": "98502500"
    },
    "fees": {
      "sourceChainGas": "180000",
      "destinationChainGas": "120000",
      "bridgeFee": "0.001",
      "totalFeesUSD": "45.32"
    },
    "timing": {
      "estimatedTime": 45000,
      "steps": [
        {"step": "Source chain swap", "estimatedTime": 15000},
        {"step": "ICP bridge", "estimatedTime": 25000},
        {"step": "Destination chain swap", "estimatedTime": 5000}
      ]
    }
  }
}
```

#### Supported Cross-Chain Pairs
```http
GET /api/quote/cross-chain/supported-pairs
```

```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain/supported-pairs" | jq
```

#### Cross-Chain Batch Processing
```http
POST /api/quote/cross-chain/batch
```

```bash
curl -X POST https://quoter.appicdao.com/api/quote/cross-chain/batch \
  -H "Content-Type: application/json" \
  -d '{
    "quotes": [
      {
        "tokenA": "0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d",
        "chainA": "56",
        "tokenB": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
        "chainB": "8453",
        "amount": "100000000"
      }
    ]
  }' | jq
```

#### Cross-Chain Health Check
```http
GET /api/quote/cross-chain/health
```

```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain/health" | jq
```

### 🔗 ICP Integration Endpoints

#### ICP-Only Quote
```http
GET /api/icp/quote?tokenIn={principal}&tokenOut={principal}&amountIn={amount}
```

**Example:**
```bash
curl "https://quoter.appicdao.com/api/icp/quote?tokenIn=rdmx6-jaaaa-aaaah-qcaiq-cai&tokenOut=rrkah-fqaaa-aaaah-qcaiq-cai&amountIn=1000000000000000000" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "protocol": "icp",
    "tokenIn": "rdmx6-jaaaa-aaaah-qcaiq-cai",
    "tokenOut": "rrkah-fqaaa-aaaah-qcaiq-cai",
    "amountIn": "1000000000000000000",
    "amountOut": "1855340000",
    "executionPrice": "1855.34",
    "path": ["WETH", "USDC"],
    "score": 97.2,
    "estimatedTime": 3000,
    "minAmountOut": "1836886700",
    "slippage": "1.0%"
  }
}
```

#### ICP Pool Information
```http
GET /api/icp/pools
```

**With specific token pair:**
```bash
curl "https://quoter.appicdao.com/api/icp/pools?tokenIn=rdmx6-jaaaa-aaaah-qcaiq-cai&tokenOut=rrkah-fqaaa-aaaah-qcaiq-cai" | jq
```

**All pools overview:**
```bash
curl "https://quoter.appicdao.com/api/icp/pools" | jq
```

#### Supported ICP Tokens
```http
GET /api/icp/tokens
```

```bash
curl "https://quoter.appicdao.com/api/icp/tokens" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "supportedTokens": [
      "rdmx6-jaaaa-aaaah-qcaiq-cai",
      "rrkah-fqaaa-aaaah-qcaiq-cai",
      "ryjl3-tyaaa-aaaah-qcaia-cai",
      "xkbra-raaaa-aaaah-qcaiq-cai"
    ],
    "totalTokens": 15,
    "totalPairs": 42,
    "pairs": [
      {
        "tokenIn": "rdmx6-jaaaa-aaaah-qcaiq-cai",
        "tokenOut": "rrkah-fqaaa-aaaah-qcaiq-cai",
        "hasDirectPool": true,
        "hasMultiHopPath": true,
        "hasAnyRoute": true
      }
    ]
  }
}
```

#### ICP Health Check
```http
GET /api/icp/health
```

```bash
curl "https://quoter.appicdao.com/api/icp/health" | jq
```

#### ICP Route Testing
```http
POST /api/icp/test
```

```bash
curl -X POST https://quoter.appicdao.com/api/icp/test \
  -H "Content-Type: application/json" \
  -d '{
    "tokenIn": "rdmx6-jaaaa-aaaah-qcaiq-cai",
    "tokenOut": "rrkah-fqaaa-aaaah-qcaiq-cai",
    "amount": "1000000000000000000",
    "exactIn": true
  }' | jq
```

#### ICP Batch Quotes
```http
POST /api/icp/batch
```

```bash
curl -X POST https://quoter.appicdao.com/api/icp/batch \
  -H "Content-Type: application/json" \
  -d '{
    "pairs": [
      {
        "tokenIn": "rdmx6-jaaaa-aaaah-qcaiq-cai",
        "tokenOut": "rrkah-fqaaa-aaaah-qcaiq-cai",
        "amountIn": "1000000000000000000"
      }
    ]
  }' | jq
```

---

## 📊 System Monitoring

### Health Check Endpoints

#### Main Health Check
```http
GET /health
```

```bash
curl "https://quoter.appicdao.com/health" | jq
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-07-15T10:30:00.000Z",
  "service": "DEX Aggregator API"
}
```

#### Detailed Quote Health
```http
GET /api/quote/health
```

```bash
curl "https://quoter.appicdao.com/api/quote/health" | jq
```

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "healthy",
    "timestamp": "2025-07-15T10:30:00.000Z",
    "services": {
      "evm": {
        "status": "healthy",
        "chains": {
          "1": { "quotersInitialized": 3 },
          "56": { "quotersInitialized": 2 },
          "8453": { "quotersInitialized": 3 },
          "42161": { "quotersInitialized": 3 }
        }
      },
      "icp": {
        "status": "healthy",
        "enabled": true,
        "supportedTokens": 15,
        "poolsInCache": 42,
        "lastUpdate": "2025-07-15T10:29:45.000Z"
      }
    },
    "summary": {
      "totalChains": 5,
      "totalProtocols": 4,
      "icpEnabled": true
    }
  }
}
```

---

## 💡 Complete Testing Examples

### 1. Regular Single-Chain Quotes (/api/quote)

#### Basic EVM Quote:
```bash
curl "https://quoter.appicdao.com/api/quote?tokenIn=0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48&tokenOut=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&amountIn=1000000&chainId=1" | jq
```

#### Native Token Input (ETH → USDC):
```bash
curl "https://quoter.appicdao.com/api/quote?tokenIn=0x0000000000000000000000000000000000000000&tokenOut=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&amountIn=100000000000000000&chainId=8453" | jq
```

#### Native Token Output (USDC → ETH):
```bash
curl "https://quoter.appicdao.com/api/quote?tokenIn=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&tokenOut=0x0000000000000000000000000000000000000000&amountIn=100000000&chainId=8453" | jq
```

### 2. Cross-Chain Quotes (/api/quote/cross-chain)

#### Basic Cross-Chain:
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000" | jq
```

#### Native Token Cross-Chain (BNB → USDC):
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x0000000000000000000000000000000000000000&chainA=56&tokenB=0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913&chainB=8453&amount=100000000000000000" | jq
```

#### Cross-Chain to Native (USDC → ETH):
```bash
curl "https://quoter.appicdao.com/api/quote/cross-chain?tokenA=0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d&chainA=56&tokenB=0x0000000000000000000000000000000000000000&chainB=8453&amount=50000000000000000000" | jq
```

---

## 🔧 Advanced Configuration

### Gas Configuration
Dynamic gas estimation with safety buffers:

```typescript
// Cross-chain gas calculation
const bridgeGas = BigNumber.from('80000');
const contractOverhead = BigNumber.from('56247');
const safetyBuffer = 50; // 50% buffer
```

### Slippage Configuration
Multi-layer slippage protection:

- **Single-chain swaps**: 0.5% default
- **Cross-chain swaps**: 1.0% default (cumulative)
- **ICP DEX swaps**: 1.0% default
- **High volatility pairs**: Up to 3.0%

### RPC Configuration
Adaptive RPC management with automatic failover:

```typescript
// Automatic RPC selection based on latency and success rate
const provider = adaptiveRpcProvider.getBestProvider(chainId);
```

---

## 🛡️ Error Handling & Status Codes

### HTTP Status Codes

| Code | Status | Description |
|------|--------|-------------|
| **200** | Success | Request completed successfully |
| **400** | Bad Request | Invalid request parameters |
| **404** | Not Found | No routes found for token pair |
| **408** | Request Timeout | Route calculation exceeded time limit |
| **503** | Service Unavailable | Required service temporarily unavailable |
| **500** | Internal Server Error | Unexpected server error |

### Common Error Responses

#### Request Timeout
```json
{
  "success": false,
  "error": "Request timed out",
  "message": "Route calculation took too long"
}
```

#### No Routes Found
```json
{
  "success": false,
  "error": "No routes found for this token pair",
  "searchOptions": {
    "chainId": 1,
    "includeICP": false,
    "protocols": ["uniswap_v2", "uniswap_v3"]
  }
}
```

#### Invalid Parameters
```json
{
  "success": false,
  "error": "Missing required parameters: tokenIn, tokenOut, amountIn"
}
```

#### Cross-Chain Errors
```json
{
  "success": false,
  "error": "Cross-chain route not supported for this chain pair",
  "chainA": { "id": 1, "name": "Ethereum" },
  "chainB": { "id": 137, "name": "Polygon" }
}
```

#### ICP Service Errors
```json
{
  "success": false,
  "error": "ICP quoter not available"
}
```

---

## 🚀 Performance Optimization

### Request Optimization
- **Batch Requests**: Use batch endpoints for multiple quotes
- **Parameter Optimization**: Only include necessary parameters
- **Caching**: Implement client-side caching for repeated requests

### Response Time Optimization
- **Specific Chains**: Specify chainId to avoid unnecessary searches
- **Protocol Filtering**: Use `protocols` parameter to limit search scope

### Performance Metrics
- **Response Time**: Target <500ms for single-chain, <2s for cross-chain
- **Success Rate**: >99% uptime
- **RPC Calls**: Optimized with intelligent caching
- **Gas Accuracy**: ±5% of actual gas usage

---

## 📞 Support & Resources

### Documentation
- **API Documentation**: https://quoter.appicdao.com/api-docs
- **GitHub Repository**: https://github.com/Appic-Solutions/smart-routing
- **Integration Examples**: Available in the repository

### Support Channels
- **GitHub Issues**: Report bugs and feature requests
- **Technical Documentation**: Comprehensive API reference
- **Community Support**: Developer community discussions

### Key Features Summary
- ✅ **Multi-chain support** across 5+ networks
- ✅ **Zero gas fees** on ICP routes
- ✅ **Cross-chain capabilities** via ICP bridge
- ✅ **Real-time optimization** with intelligent routing
- ✅ **Comprehensive error handling** and monitoring
- ✅ **High performance** with sub-second response times
- ✅ **Enterprise-grade reliability** with 99%+ uptime