# MigrationBridge

A cross-chain token migration bridge for ZORIA tokens from **Solana to BNB Smart Chain (BSC)**, built on Solana and **BSC** with EVM compatibility.

## Technology Stack

- **Blockchain**: Solana + BNB Smart Chain (BSC)
- **Backend**: Node.js, Express, ethers.js, @solana/web3.js
- **Frontend**: Vanilla JS, Solana wallet adapter
- **Database**: SQLite (migrations.db)

## Supported Networks

- **Solana Mainnet** — Source chain for ZORIA tokens
- **BNB Smart Chain Mainnet** (Chain ID: 56) — Destination chain for ZORIA tokens

## Contract Addresses

| Network | Token Contract |
|---------|----------------|
| Solana Mainnet | `9ivAqqyrQiSTa3sgV7K8jLeVNVU64StEBzuuR6Fgpump` (SPL Token Mint) |
| BNB Mainnet | `0x0B71296D09B5aa459c6c79A425e41Aa9179D7777` ($ZORIA ERC-20) |

## Features

- **Cross-chain migration** — Solana → BSC at 4:1 rate (4 Solana tokens = 1 BSC token)
- **On-chain verification** — Validates Solana transfer before sending BSC tokens
- **Relayer architecture** — Backend verifies TX and distributes BSC tokens
- **Low-cost on BSC** — Gas-efficient ERC-20 transfers on BNB Smart Chain
- **REST API** — Config, migrate, status, recent migrations

## Quick Start

### 1. Install

```bash
npm install
```

### 2. Configure

```bash
cp .env.example .env
```

Edit `.env`: set `SOLANA_VAULT_ADDRESS` (receives Solana tokens) and `BSC_PRIVATE_KEY` (BSC wallet that holds tokens to distribute).

### 3. Run

```bash
npm start
```

Server runs on `http://localhost:3000`.

## Architecture

```
Frontend (static)  →  Express API  →  Relayer
                                       ├── Verifies Solana TX
                                       └── Sends BSC tokens
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/config` | Bridge configuration |
| POST | `/api/migrate` | Register a migration |
| GET | `/api/status/:solanaTx` | Check migration status |
| GET | `/api/recent` | Recent migrations |

## VPS Deployment

```bash
# Install Node.js 18+
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Clone and setup
git clone https://github.com/JapsterD/MigrationBridge.git
cd MigrationBridge
npm install
cp .env.example .env
nano .env  # fill in keys

# Run with PM2
npm install -g pm2
pm2 start backend/server.js --name zoria-bridge
pm2 save
pm2 startup
```

Use Nginx as reverse proxy for HTTPS.
