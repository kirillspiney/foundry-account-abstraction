# Foundry Account Abstraction

A **Foundry** (Forge) project demonstrating **two types of Account
Abstraction**:

1.  **Ethereum-style Account Abstraction (ERC‑4337)**\
2.  **zkSync-native Account Abstraction**, which uses native zkSync
    validation rules and differs significantly from ERC‑4337

This repository includes contracts, scripts, and tests for both
approaches---allowing you to compare their validation flows, gas models,
and architectural differences.

------------------------------------------------------------------------

## Table of Contents

1.  What is Account Abstraction?\
2.  Two Types of Account Abstraction\
3.  Project Overview\
4.  Architecture\
5.  Features\
6.  Getting Started\
7.  Usage\
8.  Testing\
9.  Deployment\
10. Security Considerations\
11. Contributing\
12. License

------------------------------------------------------------------------

## What is Account Abstraction?

Account Abstraction allows smart contract wallets to act as first‑class
transaction initiators. Instead of relying solely on EOA private key
signatures, a smart contract account defines its own validation logic,
gas payment scheme, and execution flow.

This provides features such as:

-   Custom signature schemes\
-   Social recovery\
-   Sponsored gas via paymasters\
-   Batched transactions\
-   Non‑key‑based authentication logic

------------------------------------------------------------------------

## Two Types of Account Abstraction

This repository implements **two completely different AA models**:

### 1. Ethereum Account Abstraction --- ERC‑4337

-   Uses a **UserOperation** struct\
-   A central **EntryPoint** contract validates & executes operations\
-   Requires bundlers\
-   Does *not* modify Ethereum protocol rules\
-   Validation logic lives inside the wallet (`validateUserOp`)

### 2. zkSync Native Account Abstraction

-   Account Abstraction is **built into zkSync protocol**\
-   Every account is *by definition* a smart contract account\
-   No EntryPoint contract\
-   No ERC‑4337 UserOperation\
-   Validation logic is handled through zkSync's native account rules\
-   Gas model & calldata compression are different\
-   zkSync uses its own compiler constraints and execution pipeline

⚠️ Because zkSync uses a separate compiler and stack model, contracts
may require changes in variable layouts, stack depth, and memory usage.

This repo contains examples and tests illustrating both flows.

------------------------------------------------------------------------

## Project Overview

This repository includes:

-   An **ERC‑4337 smart wallet**\
-   An **EntryPoint-like contract**\
-   A **zkSync-style account contract** adhering to zkSync validation
    rules\
-   Scripts to deploy, run, and test both systems\
-   Forge tests for comparing logic and behavior

------------------------------------------------------------------------

## Architecture

### ERC‑4337 Flow (Ethereum)

    User → Bundler → EntryPoint → Smart Account → Execution

### zkSync Native AA

    User → zkSync Node → Smart Account → Execution (no EntryPoint)

------------------------------------------------------------------------

## Features

-   Ethereum ERC‑4337 wallet & EntryPoint\
-   zkSync native AA wallet\
-   Unified Foundry testing environment\
-   Example scripts for sending operations\
-   Demonstrations of signature validation, nonces, and execution logic\
-   Deployment scripts for both Ethereum RPC and zkSync RPC

------------------------------------------------------------------------

## Getting Started

### Prerequisites

-   **Foundry**\
-   **Rust / zkSync dependencies** if compiling zkSync contracts\
-   **Private key** for deployment\
-   Node with Ethereum or zkSync RPC

### Setup

``` bash
git clone https://github.com/kirillspiney/foundry-account-abstraction.git
cd foundry-account-abstraction
```

Install Foundry:

``` bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

------------------------------------------------------------------------

## Usage

### Build

``` bash
forge build
```

### Test

``` bash
forge test
```

### Send UserOp (Ethereum ERC‑4337)

``` bash
forge script script/SendUserOp.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

### zkSync Deployment / Tests

You may need to enable `--zksync` compiler mode:

``` bash
forge build --zksync
forge test --zksync
```

------------------------------------------------------------------------

## Deployment

### Ethereum

``` bash
forge script script/Deploy.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

### zkSync

Scripts don't work properly on zkSync at the moment.

------------------------------------------------------------------------

## Security Considerations

-   Strict validation required for all signature and nonce checks\
-   zkSync accounts must follow native validation requirements\
-   ERC‑4337 EntryPoint must properly enforce verification\
-   Test all edge cases, especially differences in stack depth or
    calldata formats

------------------------------------------------------------------------

## Contributing

PRs welcome!\
You can help by:

-   Adding more examples for zkSync\
-   Benchmarking differences between Ethereum and zkSync AA\
-   Improving tests or adding fuzz/invariant testing

------------------------------------------------------------------------

## License

MIT
