# 🌊 WattShare AMM - STX/sBTC Liquidity Pool

## 📖 Overview

WattShare AMM is an **Automated Market Maker** smart contract built on Stacks that enables decentralized swapping between STX and sBTC tokens. Using the constant product formula (`x * y = k`), this contract allows users to trade instantly without order books while enabling liquidity providers to earn fees.

## ✨ Features

- 💱 **Instant Swaps** - Trade STX for sBTC and vice versa with automated pricing
- 💧 **Liquidity Provision** - Deposit both tokens to earn trading fees
- 🎟️ **LP Tokens** - Receive fungible LP tokens representing your pool share
- 📊 **Constant Product Formula** - Fair pricing based on the `x * y = k` algorithm
- 🛡️ **Slippage Protection** - Set minimum output amounts to protect against price movements
- ⚙️ **Configurable Fees** - Contract owner can adjust the trading fee rate

## 🚀 Core Functions

### Add Liquidity

Deposit STX and sBTC to the pool and receive LP tokens proportional to your contribution.

```clarity
(add-liquidity (stx-amount uint) (sbtc-amount uint))
```

**Returns:** Amount of LP tokens minted

### Remove Liquidity

Burn your LP tokens to withdraw your share of STX and sBTC from the pool.

```clarity
(remove-liquidity (lp-amount uint))
```

**Returns:** `{stx: uint, sbtc: uint}` - Amounts withdrawn

### Swap STX for sBTC

Trade STX for sBTC with automatic pricing and slippage protection.

```clarity
(swap-stx-for-sbtc (stx-amount uint) (min-sbtc-out uint))
```

**Returns:** Amount of sBTC received

### Swap sBTC for STX

Trade sBTC for STX with automatic pricing and slippage protection.

```clarity
(swap-sbtc-for-stx (sbtc-amount uint) (min-stx-out uint))
```

**Returns:** Amount of STX received

## 📊 Read-Only Functions

Query contract state without making transactions:

- `(get-stx-reserve)` - Get current STX reserve
- `(get-sbtc-reserve)` - Get current sBTC reserve
- `(get-total-lp-supply)` - Get total LP tokens minted
- `(get-fee-rate)` - Get current fee rate (basis points)
- `(get-lp-balance (account principal))` - Get LP token balance for an account
- `(calculate-stx-to-sbtc (stx-amount uint))` - Preview STX → sBTC swap output
- `(calculate-sbtc-to-stx (sbtc-amount uint))` - Preview sBTC → STX swap output

## 🎓 Educational Concepts

### Constant Product Formula (x * y = k)

The AMM maintains a constant product of reserves:

```
STX_reserve × sBTC_reserve = k (constant)
```

When you swap tokens, the product remains constant, automatically adjusting prices based on supply and demand.

### LP Token Minting

When adding liquidity:
- **First liquidity provider:** Receives `sqrt(stx_amount × sbtc_amount)` LP tokens
- **Subsequent providers:** Receive LP tokens proportional to their contribution

### Trading Fees

Default fee: **0.3%** (30 basis points)
- Fees are deducted from input amounts before calculating swaps
- All fees remain in the pool, benefiting LP token holders

## ⚙️ Usage Instructions

### Initial Setup

1. Deploy the contract to Stacks blockchain
2. First liquidity provider calls `add-liquidity` with initial STX and sBTC amounts

### For Liquidity Providers

**Provide Liquidity:**
```clarity
(contract-call? .wattshare-amm add-liquidity u1000000 u500000)
```

**Remove Liquidity:**
```clarity
(contract-call? .wattshare-amm remove-liquidity u100000)
```

### For Traders

**Buy sBTC with STX:**
```clarity
(contract-call? .wattshare-amm swap-stx-for-sbtc u1000000 u450000)
```

**Buy STX with sBTC:**
```clarity
(contract-call? .wattshare-amm swap-sbtc-for-stx u500000 u900000)
```

## 🔒 Security Features

- ✅ Slippage protection via minimum output amounts
- ✅ Zero-amount transaction prevention
- ✅ Liquidity checks before swaps
- ✅ Owner-only fee adjustment
- ✅ Balance validation for LP token operations

## 📝 Error Codes

- `u100` - Owner-only function
- `u101` - Insufficient balance
- `u102` - Invalid amount
- `u103` - Slippage tolerance exceeded
- `u104` - Insufficient liquidity in pool
- `u105` - Zero amount not allowed
- `u106` - Invalid pool state

## 🛠️ Development

This contract is built with **Clarinet** and follows Clarity best practices for DeFi applications.

### Testing Scenarios

1. Add initial liquidity
2. Perform multiple swaps to test pricing
3. Add more liquidity to test LP token calculations
4. Remove liquidity to verify withdrawals
5. Test slippage protection with minimum output amounts

## 📄 License

MIT License - Feel free to use, modify, and distribute

## 🤝 Contributing

Contributions welcome! This is an educational MVP demonstrating core AMM concepts.

---

**Built with ❤️ for the Stacks ecosystem**