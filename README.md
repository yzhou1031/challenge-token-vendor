# Challenge: Token Vendor
> A decentralized token vending machine — the simplest form of an automated market maker

[![Solidity](https://img.shields.io/badge/Solidity-0.8.20-363636?logo=solidity&logoColor=white)]()
[![Foundry](https://img.shields.io/badge/Built_with-Foundry-red)]()
[![Next.js](https://img.shields.io/badge/Frontend-Next.js-black?logo=next.js&logoColor=white)]()
[![Sepolia](https://img.shields.io/badge/Network-Sepolia-8A2BE2)]()

🔗 [Live Demo](https://nextjs-9w5yyad7j-yuchenzhou1031-6631s-projects.vercel.app/) · 📋 [Speedrun Ethereum](https://speedrunethereum.com)

## What It Does

An ERC-20 token ("Gold" / GLD) and a Vendor contract that sells and buys back tokens at a fixed rate of 100 GLD per ETH. Users can purchase tokens by sending ETH, transfer tokens to other addresses, and sell tokens back to the Vendor using the ERC-20 `approve`/`transferFrom` pattern. The owner can withdraw accumulated ETH.

## Real-World Relevance

- **Uniswap** — started as a constant-product AMM, conceptually a more sophisticated version of this Vendor that uses a pricing curve instead of a fixed rate; the core idea (contract holds reserves, trades are trustless) is identical
- **ERC-20 approve pattern** — the two-step `approve` then `transferFrom` flow implemented in `sellTokens` is the universal mechanism for contract-to-contract token transfers across all of DeFi
- **Token distribution** — selling through a Vendor at a fixed rate is the simplest token distribution mechanism; real projects extend this with bonding curves, auctions, and vesting schedules

## Contract Architecture

| Contract | Role |
|---|---|
| `YourToken.sol` | ERC-20 token ("Gold" / GLD), mints 1000 tokens to deployer in constructor |
| `Vendor.sol` | Buys tokens with ETH at `tokensPerEth = 100`; sells tokens back; owner can `withdraw()` ETH |

## Key Concepts

- **ERC-20 approve/transferFrom for buybacks** — selling tokens requires two transactions: `yourToken.approve(vendorAddress, amount)` then `vendor.sellTokens(amount)`; the Vendor pulls tokens via `transferFrom`
- **`transfer` vs `transferFrom`** — `buyTokens` uses `transfer` (Vendor sends its own tokens); `sellTokens` uses `transferFrom` (Vendor pulls tokens from the user's allowance)
- **`call` over `transfer` for ETH** — ETH is sent with low-level `call` to avoid gas limit issues introduced by the 2300-gas stipend in `transfer`

## Local Setup

```bash
yarn chain    # start local Anvil blockchain
yarn deploy   # deploy YourToken + Vendor
yarn start    # frontend at http://localhost:3000
```
