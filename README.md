<p align="center">
  <img src="assets/social-preview.svg" alt="AssetAtlas banner" width="640" />
</p>

# AssetAtlas Backend

> Structured Stellar asset metadata and verification — off-chain indexing/API service.

![Status: v0.1.0](https://img.shields.io/badge/version-v0.1.0-blue)
![Status: not audited](https://img.shields.io/badge/audit-not%20audited-orange)
![License: Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-green)
![Stack: Node.js + Stellar](https://img.shields.io/badge/stack-Node.js%20%2B%20Stellar-purple)

> **Status note:** v0.1.0 development baseline — **not audited and not production-ready.**

## Why this exists

AssetAtlas provides structured, verifiable metadata for Stellar assets. This repository is the **off-chain indexing/API service** of the three-repo AssetAtlas system. It stays out of consensus-critical logic; on-chain state is the source of truth.

| Repo | Role |
| --- | --- |
| [assetatlas-contracts](https://github.com/stellar-assetatlas/assetatlas-contracts) | On-chain Soroban state and authorization |
| [assetatlas-app](https://github.com/stellar-assetatlas/assetatlas-app) | User-facing web application |
| **assetatlas-backend** (this repo) | Off-chain indexing/API and operational services |

## Features

- Minimal Node.js HTTP service (no framework dependencies).
- `GET /health` — liveness endpoint returning `{ ok: true, service: "assetatlas-backend" }`.
- `GET /network` — reports the configured Stellar network (testnet).
- Uses `@stellar/stellar-sdk`; indexing and persistence hooks are in place via `DATABASE_URL` for future growth.
- TypeScript with `tsc` build and `node --test` tests (`src/server.test.ts`).

## Architecture

```mermaid
flowchart LR
    A[assetatlas-app<br/>Next.js] -- BACKEND_URL --> B[assetatlas-backend<br/>HTTP API]
    B -- reads chain state --> R[Stellar RPC<br/>Soroban testnet]
    R --> C[assetatlas-contracts<br/>on-chain state]
    B -. persistence .-> D[(DATABASE_URL)]
```

## Tech stack

| Layer | Technology |
| --- | --- |
| Runtime | Node.js (ESM) |
| Language | TypeScript 5.8 (`tsx` for dev, `tsc` for build) |
| Blockchain | Stellar / Soroban, `@stellar/stellar-sdk` 17 |
| Testing | `node --test` |

## Project structure

```text
assetatlas-backend/
├── src/
│   ├── server.ts       # HTTP service (health, network)
│   └── server.test.ts  # Tests
├── assets/             # Banner and logo
└── .github/            # CI workflow, CODEOWNERS
```

## Prerequisites

- Node.js ≥ 20
- npm

## Installation

```bash
npm install
```

## Environment variables

Copy `.env.example` to `.env` and fill in the values:

| Variable | Description |
| --- | --- |
| `PORT` | HTTP port the service listens on (default `8787`). |
| `STELLAR_RPC_URL` | Soroban RPC endpoint, e.g. `https://soroban-testnet.stellar.org`. |
| `DATABASE_URL` | Persistence connection string (empty is fine for the current in-memory baseline). |
| `CONTRACT_ID` | Deployed AssetAtlas contract ID the backend reads from. |

## Running locally

```bash
npm run dev
```

Then check <http://localhost:8787/health>.

## Testing

```bash
npm test
```

## Building

```bash
npm run build   # tsc — outputs compiled JS
```

## Roadmap

- [ ] Implement real indexing of AssetAtlas contract events.
- [ ] Add persistence behind `DATABASE_URL`.
- [ ] Expose asset-metadata API endpoints consumed by assetatlas-app.
- [ ] Independent security review (project is unaudited).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Security

See [SECURITY.md](SECURITY.md). **This project is unaudited** — do not use in production.

## Code of Conduct

See [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Maintainer

**Hikmaholadele** — [@Hikmaholadele](https://github.com/Hikmaholadele)

## License

[Apache-2.0](LICENSE)
