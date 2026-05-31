# StellarPaymentRouter Organization README

## Table of Contents

1. [Overview](#overview)
2. [The Challenge](#the-challenge)
3. [The Solution](#the-solution)
4. [Architecture](#architecture)
5. [Repository Structure](#repository-structure)
6. [Getting Started](#getting-started)
7. [Contributing](#contributing)
8. [Security](#security)
9. [License](#license)

---

## Overview

StellarPaymentRouter (SPR) is a decentralized payment routing protocol built on the Stellar blockchain. SPR automatically discovers and executes optimal payment routes across multiple liquidity pools, enabling efficient multi-asset transactions on Stellar.

SPR abstracts the complexity of cross-asset payments by providing intelligent pathfinding, real-time liquidity aggregation, and atomic transaction execution. Whether users need to exchange assets directly or through intermediate hops, SPR handles all routing decisions automatically.

### Key Capabilities

* Intelligent pathfinding across multiple liquidity pools
* Multi-hop route support with fee optimization
* Slippage protection and real-time pool discovery
* Atomic transaction execution (all-or-nothing)
* SDK for programmatic integration
* User-friendly web interface

---

## The Challenge

Current Stellar payment flows present several critical limitations:

### Limited Direct Pairs

Most Stellar assets do not have direct trading pairs. Users attempting to exchange Asset A for Asset B may find no direct pool available, requiring manual intervention and complex workarounds.

### Manual Route Discovery

When direct routes do not exist, users must manually identify intermediate assets, calculate optimal paths, and execute multiple sequential transactions.

This process is:

* Time-consuming and error-prone
* Subject to price slippage across multiple transactions
* Incurs cumulative fees for each intermediate swap
* Requires deep technical knowledge of DEX protocols

### Inefficient Fee Structure

Without intelligent routing, users often overpay fees and receive suboptimal execution prices. Sequential transactions multiply fees and increase overall transaction costs.

### Poor User Experience

The complexity of manual routing creates friction for mainstream adoption. Non-technical users cannot efficiently navigate multi-hop swaps, limiting Stellar adoption for payment use cases.

---

## The Solution

StellarPaymentRouter solves these challenges through:

### Automatic Route Discovery

SPR continuously analyzes available liquidity pools and discovers all possible routes between any two assets, including multi-hop paths up to 5 intermediate swaps.

### Intelligent Path Selection

SPR evaluates multiple route options and selects the optimal path based on:

* Total fees across all hops
* Liquidity depth and slippage impact
* Current exchange rates and pool reserves
* Route efficiency score

### Atomic Execution

SPR uses Soroban smart contracts to execute entire routes as single transactions. If any hop fails, the entire transaction reverts (all-or-nothing), eliminating counterparty risk.

### Real-Time Liquidity Aggregation

SPR aggregates liquidity from multiple DEX protocols on Stellar, providing access to the deepest pools and best rates available at transaction time.

### Developer-Friendly Integration

SPR provides:

* TypeScript/JavaScript SDK for programmatic access
* REST API for route discovery and execution
* Type-safe interfaces and comprehensive documentation
* React hooks for frontend integration

---

## Architecture

### High-Level Flow

```text
User/Application
        |
        v
SPR Frontend (UI)
        or
SPR SDK (Programmatic)
        |
        v
SPR Core Backend
(Route Discovery & Optimization)
        |
        +-- Stellar Horizon
        |   (Account & Network Data)
        |
        +-- Soroban RPC
        |   (Smart Contract Interaction)
        |
        v
SPR Contracts
(Route Execution)
        |
        v
Stellar Network
(Transaction Settlement)
```

### Four Core Components

#### 1. SPR Core (Node.js/Express)

The backend routing engine responsible for:

* Route discovery using BFS pathfinding algorithm
* Pool data aggregation and caching
* Liquidity analysis and validation
* Fee calculation and optimization
* REST API endpoints for route queries
* Transaction status tracking

**Repository:** `spr-core`

#### 2. SPR Contracts (Soroban/Rust)

Smart contracts deployed on Stellar that:

* Execute route transactions atomically
* Perform on-chain AMM calculations
* Manage liquidity pool interactions
* Collect and manage routing fees
* Emit transaction events
* Enforce access control

**Repository:** `spr-contracts`

#### 3. SPR SDK (TypeScript/JavaScript)

Client library providing:

* Type-safe interfaces for route discovery
* Wallet integration (Freighter support)
* Request handling and retry logic
* Response caching
* React hooks for frontend applications
* Comprehensive type definitions

**Repository:** `spr-sdk`

#### 4. SPR Frontend (Next.js/React)

User-facing application featuring:

* Route finder interface
* Pool explorer with filtering and sorting
* Transaction history tracking
* Wallet connection and management
* Real-time price updates
* Mobile-responsive design

**Repository:** `spr-frontend`

### Technology Stack

| Component | Language   | Framework   | Runtime           |
| --------- | ---------- | ----------- | ----------------- |
| Core      | JavaScript | Express.js  | Node.js 18+       |
| Contracts | Rust       | Soroban SDK | WebAssembly       |
| SDK       | TypeScript | None        | Node.js / Browser |
| Frontend  | TypeScript | Next.js     | Node.js / Browser |

### Data Flow

1. User submits route request (source asset, destination asset, amount)
2. SPR Core queries available pools from Stellar network
3. SPR Core builds graph of liquidity pools
4. BFS algorithm discovers all possible routes
5. Routes scored and ranked by efficiency
6. Best route returned to user with estimated output and fees
7. User approves and signs transaction
8. Signed transaction submitted to SPR Contract
9. Contract executes each hop sequentially
10. Transaction settles on Stellar network
11. Final output transferred to user

---

## Repository Structure

This organization contains four primary repositories:

### spr-core

Backend routing engine and API server.

#### Responsibilities

* Route discovery and optimization
* Pool aggregation and caching
* REST API endpoints
* Stellar Horizon integration
* Soroban contract invocation

#### Key Files

* `src/services/route.service.js` — Route finding logic
* `src/services/liquidity.service.js` — Pool management
* `src/services/stellar.service.js` — Horizon integration
* `src/utils/pathfinding.js` — Pathfinding algorithm
* `docs/ARCHITECTURE.md` — Detailed architecture

**Getting Started:** See `spr-core/README.md`

### spr-contracts

Smart contracts for atomic route execution.

#### Responsibilities

* Route execution
* Fee management
* Pool interaction
* Event emission
* Access control

#### Key Files

* `src/lib.rs` — Main contract entry points
* `src/router.rs` — Routing logic
* `src/types.rs` — Type definitions
* `docs/ARCHITECTURE.md` — Contract design

**Getting Started:** See `spr-contracts/README.md`

### spr-sdk

TypeScript/JavaScript client library.

#### Responsibilities

* Type-safe API client
* Wallet integration
* Response caching
* Error handling
* React hooks

#### Key Files

* `src/client/SprClient.ts` — Main client class
* `src/types/index.ts` — Type definitions
* `src/hooks/` — React hooks
* `docs/ARCHITECTURE.md` — SDK design

**Getting Started:** See `spr-sdk/README.md`

### spr-frontend

User-facing web application.

#### Responsibilities

* Route finder UI
* Pool explorer
* Transaction management
* Wallet connection
* Real-time updates

#### Key Files

* `app/routes/page.tsx` — Route finder
* `app/pools/page.tsx` — Pool explorer
* `components/` — Reusable components
* `hooks/` — Custom hooks
* `docs/ARCHITECTURE.md` — Frontend architecture

**Getting Started:** See `spr-frontend/README.md`

---

## Getting Started

### Quick Start

### 1. Explore the Project

Start with the organization overview and architecture documentation:

```text
https://github.com/StellarPaymentRouter
```

### 2. Choose Your Path

* **Backend Developer:** Start with `spr-core/README.md`
* **Smart Contract Developer:** Start with `spr-contracts/README.md`
* **Frontend Developer:** Start with `spr-frontend/README.md`
* **SDK/Library Developer:** Start with `spr-sdk/README.md`

### 3. Install Dependencies

Each repository has its own setup process. Refer to the specific repository README and `docs/DEPLOYMENT.md` for detailed instructions.

### 4. Run Tests

Each repository includes comprehensive tests:

```bash
# In spr-core
npm test

# In spr-contracts
cargo test

# In spr-sdk
npm test

# In spr-frontend
npm test
```

### Review Documentation

Each repository includes detailed documentation in the `docs/` folder:

* `docs/ARCHITECTURE.md` — System design and component overview
* `docs/CONTRIBUTING.md` — Contribution guidelines
* `docs/DEPLOYMENT.md` — Deployment instructions
* `docs/SECURITY.md` — Security policies

### Development Environment

#### Prerequisites

* Node.js 18+ (for spr-core, spr-sdk, spr-frontend)
* Rust 1.70+ (for spr-contracts)
* npm or yarn
* Git

#### Environment Setup

Clone all repositories:

```bash
git clone https://github.com/StellarPaymentRouter/spr-core.git
git clone https://github.com/StellarPaymentRouter/spr-contracts.git
git clone https://github.com/StellarPaymentRouter/spr-sdk.git
git clone https://github.com/StellarPaymentRouter/spr-frontend.git
```

* Install dependencies in each repository (see individual README files)
* Configure environment variables (see `docs/DEPLOYMENT.md` in each repo)
* Run development servers (see individual repository instructions)

---

## Contributing

We welcome contributions from developers of all experience levels. The SPR project is organized around clear, well-defined issues across all repositories.

### Contribution Workflow

#### Choose an Issue

Browse available issues in the repository:

* `help wanted` — Good for contributors
* `good first issue` — Good for newcomers
* `enhancement` — Feature requests
* `bug` — Bug fixes

#### Review Contribution Guidelines

Each repository includes contribution guidelines:

* `docs/CONTRIBUTING.md` — Repository-specific process
* Code style and standards
* Testing requirements
* Pull request process

#### Fork and Branch

```bash
git checkout -b feature/issue-description
```

#### Develop and Test

Follow the repository's development process and run tests before submitting.

#### Submit Pull Request

Include:

* Clear description of changes
* Reference to related issue
* Test coverage
* Documentation updates

#### Code Review

Maintainers review all contributions. Be prepared for feedback and iteration.

### Contribution Guidelines

#### Code Quality

* Follow repository code style and conventions
* Write clear, self-documenting code
* Include meaningful comments for complex logic
* Add appropriate error handling
* Avoid code duplication

#### Testing

* Write tests for all new functionality
* Ensure existing tests still pass
* Target 90%+ code coverage
* Test edge cases and error scenarios

#### Documentation

* Update README files if needed
* Add JSDoc/doc comments for public APIs
* Include examples for new features
* Update relevant markdown files in `docs/`

#### Commit Messages

Use clear, descriptive commit messages:

```text
feat: add new feature description
fix: resolve issue with component
docs: update architecture documentation
test: add tests for route discovery
```

### Code of Conduct

All contributors are expected to:

* Be respectful and professional
* Provide constructive feedback
* Help others learn and grow
* Report issues through appropriate channels
* Follow open-source best practices

### Getting Help

* Review `docs/ARCHITECTURE.md` for system design
* Check existing issues and pull requests for context
* Ask questions in issue discussions
* Contact maintainers for clarification

---

## Security

Security is critical for payment routing infrastructure. We follow strict security practices across all repositories.

### Reporting Security Issues

If you discover a security vulnerability, please report it responsibly:

* Do not open a public issue or discussion
* Email security details to the maintainers privately
* Allow time for the team to assess and address the issue
* Coordinated disclosure will be arranged

See `SECURITY.md` in each repository for detailed security policies.

### Security Standards

All repositories maintain:

* Regular dependency audits
* Security-focused code review process
* Input validation and error handling
* Secure defaults for all configurations
* Protection against common vulnerabilities
* Comprehensive test coverage

### Repository-Specific Security

Each repository includes security documentation:

* `spr-core/docs/SECURITY.md` — API security, authentication, data handling
* `spr-contracts/docs/SECURITY.md` — Smart contract security, audit status
* `spr-sdk/docs/SECURITY.md` — Client security, secure storage
* `spr-frontend/docs/SECURITY.md` — Frontend security, wallet integration

### Compliance

SPR follows:

* OWASP Top 10 security practices
* Smart contract security best practices
* Stellar blockchain security guidelines
* Industry-standard encryption and hashing

---

## License

StellarPaymentRouter is open-source software released under the Apache License 2.0.

### Apache License 2.0

You are free to:

* Use the software for any purpose
* Modify and distribute the software
* Include the software in proprietary applications

With the following conditions:

* Include a copy of the license
* Include a notice of changes made
* Include attribution to the original authors
* Provide a copy of the Apache 2.0 license

See `LICENSE` file in each repository for the complete license text.

### Third-Party Licenses

SPR uses open-source dependencies.

Each repository includes:

* `package.json` (Node.js/npm dependencies)
* `Cargo.toml` (Rust dependencies)

Run the following to review licenses:

```bash
# Node.js projects
npm licenses

# Rust projects
cargo license
```

All dependencies are reviewed for license compatibility with Apache 2.0.

---

## Additional Resources

### External Resources

* [Stellar Documentation](https://developers.stellar.org/docs)
* [Soroban Smart Contracts](https://developers.stellar.org/docs/build/guides/dapps/working-with-contract-specs#introduction)
* [Horizon API Reference](https://developers.stellar.org/docs/data/apis/horizon)

### Communication

* GitHub Issues: Report bugs and request features
* GitHub Discussions: Ask questions and share ideas
* Pull Requests: Contribute code and documentation

### Support

For questions, issues, or feedback:

* Check existing issues and documentation
* Search GitHub Discussions for similar questions
* Open a new issue with detailed information
* Contact maintainers if necessary

### Roadmap

Current focus areas:

* Phase 1: Core implementation (route discovery, smart contracts, SDK)
* Phase 2: Mainnet deployment and testing
* Phase 3: Performance optimization and scaling
* Phase 4: Advanced features and ecosystem integration

See individual repository documentation for detailed roadmaps.

### Acknowledgments

StellarPaymentRouter is built on the foundation of the Stellar blockchain and benefits from the Stellar developer community.

We acknowledge:

* Stellar Development Foundation
* Soroban smart contract platform
* Open-source contributors and maintainers
* Community members and users
