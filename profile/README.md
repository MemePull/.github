# MemePull ARENA

> Gamified Liquidity Warfare — where meme coin communities compete in real-time battles and prediction markets.

[![Solidity](https://img.shields.io/badge/Solidity-^0.8.x-363636?logo=solidity)](https://soliditylang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org/)
[![Chainlink CRE](https://img.shields.io/badge/Chainlink-CRE-375BD2?logo=chainlink)](https://chain.link/)
[![World ID](https://img.shields.io/badge/World_ID-MiniKit-black?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48Y2lyY2xlIGN4PSIxMiIgY3k9IjEyIiByPSIxMCIgc3Ryb2tlPSJ3aGl0ZSIgc3Ryb2tlLXdpZHRoPSIyIi8+PC9zdmc+)](https://worldcoin.org/)
[![Network](https://img.shields.io/badge/Network-World_Chain_Sepolia-blue)](https://worldchain.world/)

<img width="1920" height="1078" alt="image" src="https://github.com/user-attachments/assets/6b22752a-6f6c-4fa9-93b1-f650d4cd543d" />

**Disclaimer**: This is a hackathon MVP running on **World Chain Sepolia testnet** only. Not audited. Not intended for production use or real financial transactions.

---

## What is MemePull ARENA?

MemePull ARENA is a Web3 GameFi platform that combines community tribalism with decentralized oracle infrastructure. Two core products:

### 1. PvP Battle (Battle of the Tribes)
Winner-takes-all liquidity competitions between two meme token communities. Users pick a side, deposit tokens, and the community with the best price performance wins the combined pool.

```
Pick Side → Deposit Tokens → Battle Begins → Price Performance Tracked → Winner Takes Pool
```

### 2. Prediction Markets
Template-based Yes/No prediction markets for verifiable on-chain milestones.

```
"Will $PEPE hit $1B market cap by March 2026?"  →  YES / NO  →  Oracle Resolves  →  Winners Claim
```

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                              │
│              Next.js 16 · React 19 · Tailwind                │
│         Retro Arcade UI · wagmi/viem · World ID SDK          │
└──────────────────────────┬──────────────────────────────────┘
                           │
                    Smart Contract Calls
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                     SMART CONTRACTS                          │
│                   World Chain Sepolia                         │
│                                                              │
│  ┌─────────────┐  ┌──────────────────┐  ┌────────────────┐  │
│  │ BattleArena │  │ PredictionMarket │  │ MulticallRouter│  │
│  └──────┬──────┘  └────────┬─────────┘  └────────────────┘  │
│         │                  │                                 │
│  ┌──────▼──────────────────▼─────┐                           │
│  │      WorldIDVerifier          │                           │
│  │   (Ghost / Device / Human)    │                           │
│  └───────────────────────────────┘                           │
└──────────────────────────┬──────────────────────────────────┘
                           │
                   CRE Report / onReport()
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                   CRE ORCHESTRATOR                           │
│              Chainlink Compute Runtime Environment            │
│                                                              │
│  ┌────────────────┐  ┌───────────────────┐                   │
│  │ Market Auditor │  │ Battle Settlement │                   │
│  │  (Cron 30s)    │  │   (Cron 5min)     │                   │
│  └────────────────┘  └───────────────────┘                   │
│  ┌────────────────┐  ┌───────────────────┐                   │
│  │   Liquidity    │  │ Market Resolution │                   │
│  │   Monitor      │  │   (Cron 15min)    │                   │
│  │  (Cron 1h)     │  │                   │                   │
│  └────────────────┘  └───────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

---

## Key Integrations

| Integration | Purpose |
| --- | --- |
| **Chainlink CRE** | Trustless workflow orchestration — market auditing, TWAP price feeds, settlement, liquidity monitoring |
| **World ID (MiniKit SDK)** | Sybil resistance via identity tiers (Ghost / Device / Human) |
| **DexScreener API** | Real-time DEX liquidity and price data for token validation |
| **Uniswap V3 (World Chain)** | Swap rkouting for deposits and settlement |

---

## World ID Tiers

| Tier | Requirement | Deposit Limit | Participation Fee | Creation Fee |
| --- | --- | --- | --- | --- |
| **Ghost** | Wallet only (no World ID) | $10 max | 1% | $30 |
| **Device** | Device-verified World ID | Unlimited | 0% | $5 |
| **Human** | Orb-verified World ID | Unlimited | 0% | $5 |

---

## Reward Distribution

| Allocation | % | Recipient |
| --- | --- | --- |
| **Winner Pool** | 90% | Winning side participants (pro-rata) |
| **WLD Victory Bonus** | 4% | Verified Humans on winning side |
| **WLD Safety Net** | 4% | Verified Humans on losing side |
| **Creator Reward** | 0.5% | Market creator (only on settlement) |
| **Protocol Treasury** | 1.5% | Protocol operations |

---

## Security Features

- **TWAP Price Integrity** — 30-minute Time-Weighted Average Price prevents flash-loan price manipulation
- **Identity Verification** — World ID tiers limit unverified participation and enforce deposit caps
- **Liquidity Monitoring** — Chainlink CRE monitors DEX liquidity hourly; >50% drop triggers safety pause with full refund
- **Token Safety Verification** — Automated contract validation before market approval
- **Atomic Transactions** — MulticallRouter ensures swap + deposit is all-or-nothing
- **Permissionless with Bond** — Creation fees deter low-quality proposals without gatekeeping

---

## Repository Structure

This project is organized as a monorepo with three main modules:

```
MemePull ARENA/
├── fe/                  # Frontend (Next.js 16 + React 19)
├── sc/                  # Smart Contracts (Solidity + Foundry)
├── orchestrator/        # Chainlink CRE Workflows (Go)
└── docs/                # Project documentation
```

| Module | Tech Stack | Description |
| --- | --- | --- |
| [`fe/`](fe/) | Next.js 16, React 19, TypeScript, Tailwind CSS, shadcn/ui | Retro arcade-themed game UI with wallet integration |
| [`sc/`](sc/) | Solidity ^0.8.x, Foundry | On-chain logic for battles, markets, identity, and routing |
| [`orchestrator/`](orchestrator/) | Go 1.22+, Chainlink CRE SDK, WASI | Automated workflows for auditing, settlement, and monitoring |

Each module has its own README with setup instructions and detailed documentation.

---

## Quick Start

### Prerequisites

- **Node.js** >= 18 + **pnpm** (frontend)
- **Foundry** (smart contracts)
- **Go** 1.22+ + **Chainlink CRE CLI** (orchestrator)

### Frontend

```bash
cd fe
pnpm install
cp .env.example .env.local   # configure contract addresses
pnpm dev                      # http://localhost:3000
```

### Smart Contracts

```bash
cd sc
forge build
forge test
```

### Orchestrator

```bash
cd orchestrator
go mod tidy
cre workflow simulate battle-proposed-workflow --target staging-settings
```

---

## Deployed Contracts — World Chain Sepolia

| Contract | Address |
| --- | --- |
| WorldIDVerifier | `0xE995A47746Ad8F47707812bEF4cAd1500b40e56C` |
| BattleArena | `0xAdB0a12e57e754c4Fa0DE48FbBe3B89C021f9979` |
| PredictionMarket | `0x552aa36835F435606A2C526707BBeC9520E4FE60` |
| MulticallRouter | `0x5dE69c87219bA05fB4b1D1f35E16C1471FB6D712` |
| Treasury | `0xe051A75a469aA875c5fED943084Fb48C82393A80` |
| CRE Forwarder | `0x6e9ee680ef59ef64aa8c7371279c27e496b5edc1` |

### Mock Tokens (Testnet)

| Token | Address | Decimals |
| --- | --- | --- |
| MockUSDC | `0xf63988f186Bc09c8437B98b554124DbbA0954A1E` | 6 |
| MockWLD | `0x45D5b1893ea6843E9eD3ac48823b7990831631D9` | 18 |
| MockPEPE | `0x56E8eDa56463B81BF02ac662799A7885cD7799ce` | 18 |
| MockSHIB | `0x4190dBd17d4719df007ED0a7b2EA0226d96e4fb4` | 18 |
| MockFLOKI | `0x42642f6326dBaEA0345e0fB5366850bb17187612` | 9 |
| MockMOG | `0x5403ff9c5c173eEe01255Eeb4d0925bD21748311` | 18 |

All mock tokens have a public `faucet()` function (1 hour cooldown per address).

---

## Tech Stack

| Layer | Technology | Notes |
| --- | --- | --- |
| **Network** | World Chain Sepolia (testnet) | World ID native support |
| **Smart Contracts** | Solidity + Foundry | 4 core contracts |
| **Orchestrator** | Chainlink CRE (Go SDK) | 4 automated workflows |
| **Frontend** | Next.js 16 (App Router) | Turbopack dev server |
| **Styling** | Tailwind CSS 3.4 + shadcn/ui | Retro arcade theme |
| **Identity** | World ID MiniKit SDK | Zero-knowledge proof verification |
| **Package Manager** | pnpm | Frontend dependencies |

---

## Team

Built for the **Chainlink Hackathon** by the MemePull team.

---

## License

This project was built for a hackathon. All code is provided as-is for educational and demonstration purposes.

**This software is not audited and should not be used in production or with real funds.**
