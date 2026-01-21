# 1inch Data Engineer Interview Glossary

## 1inch Specific Terms

**Fusion Mode**
1inch's gasless swap protocol based on a "Dutch Auction" model. Users sign an order, and professional market makers (Solvers) compete to fill it. The price starts high and decreases over time until a Solver accepts it.

**Pathfinder**
1inch’s proprietary aggregation algorithm that finds the most efficient route for a token swap. It splits trades across multiple DEXs (Decentralized Exchanges) and liquidity depths to minimize slippage and gas fees.

**Solver**
A professional market maker or bot in the 1inch Fusion ecosystem that listens for user orders and fills them. Solvers pay the gas fees on behalf of the user and profit from arbitrage or liquidity management.

## Blockchain Data Fundamentals

**ABI (Application Binary Interface)**
The standard interface for interacting with smart contracts. In data engineering, it functions like a "schema" or "decoder ring," telling you how to interpret the raw hexadecimal data of transaction inputs and event logs.

**Archive Node**
A blockchain node that stores the entire history of the chain state, including historical states needed to replay transactions. Essential for data engineers who need to trace internal transactions or look up historical balances that standard "Full Nodes" might prune.

**Event Logs (Topics)**
Data structures emitted by smart contracts (e.g., a `Transfer` event). Data engineers "scrape" or "index" these logs to build tables of what happened on-chain.
*   *Topic 0* is usually the hash of the event signature.
*   *Topics 1-3* are indexed parameters (searchable).

**Internal Transaction (Trace)**
Value transfers (like ETH) that happen *inside* a smart contract execution but are not the main top-level transaction. These do not always generate an event log, making them harder to index without specialized tools (like `debug_traceTransaction`).

**Mempool**
The "waiting room" for transactions. It contains pending transactions that users have broadcast but miners/validators have not yet included in a block. Analyzing this allows for real-time predictive data but is highly volatile.

**Reorg (Chain Reorganization)**
A situation where the blockchain "changes its mind" about which block is valid. This happens when two miners find a block simultaneously. A data pipeline must be able to detect when a block it previously indexed has been "orphaned" (rejected) and delete/rewrite that data.

**RPC (Remote Procedure Call)**
The protocol used to communicate with a blockchain node (e.g., `eth_getBlockByNumber`). It is the rawest way to fetch data.

**Uint256**
The standard integer type in Ethereum (256-bit unsigned integer). It can hold massive numbers (up to 10^77). Most standard data warehouses (Snowflake, BigQuery) choke on this precision, requiring engineers to store them as strings or perform custom handling.

## Trading & DeFi Mechanics

**Dutch Auction**
A market structure where the price of an asset starts high and progressively drops until a buyer accepts it. 1inch Fusion uses this to ensure users get a fair market price without worrying about setting gas fees.

**MEV (Maximal Extractable Value)**
Profit that miners or bots extract by reordering, including, or censoring transactions within a block. Common forms include arbitrage and sandwich attacks. 1inch Fusion aims to protect users from toxic MEV.

**Sandwich Attack**
A predatory MEV strategy where a bot spots a user's large buy order in the mempool, buys the token *before* them (pushing the price up), and sells *immediately after* them for a guaranteed profit.

**Slippage**
The difference between the expected price of a trade and the price at which the trade is actually executed. High slippage often occurs in pools with low liquidity or during high volatility.

**TVL (Total Value Locked)**
A metric representing the total amount of assets currently staked or deposited in a specific DeFi protocol or liquidity pool. It is a standard proxy for a protocol's health and usage.

**Wash Trading**
A form of market manipulation where an entity buys and sells the same asset repeatedly to create misleading artificial activity (volume). Data engineers often need to write algorithms to filter this out of analytics.

## Network Mechanics & Economics

**Gas / Gas Fees**
The "fuel" required to execute operations on the Ethereum network. Every computation (like a swap or a transfer) requires a certain amount of processing power.
*   **Gas Limit:** The maximum amount of gas a user is willing to spend on a transaction (e.g., "I will use up to 100,000 units").
*   **Gas Price:** The cost per unit of gas, usually measured in *Gwei*.
*   *Relevance to Data:* High gas prices often correlate with high network congestion and slippage. 1inch Fusion optimizes this by having Solvers pay the gas, not users.

**Gwei**
A denomination of Ether used to measure gas prices.
*   1 Gwei = 0.000000001 ETH (10^-9 ETH).
*   *Wei* is the smallest possible unit of Ether (10^-18 ETH).

**Nonce**
A counter associated with every account (wallet) that tracks how many transactions it has sent.
*   *Purpose:* Prevents "replay attacks" (sending the same transaction twice).
*   *Data Engineering Note:* When indexing transactions, ordering them by `nonce` is crucial to reconstructing the correct history of an account, especially if timestamps are identical in the same block.

## Cryptography & Identity

**Hash (Keccak-256)**
A one-way function that turns any amount of data into a unique fixed-length string (like a fingerprint).
*   *Transaction Hash (TxHash):* The unique ID of a transaction (e.g., `0x123...`).
*   *Block Hash:* The fingerprint of an entire block, linking it to the previous one.
*   *Relevance:* Hashes are the primary keys in almost all blockchain data tables.

**Wallet (EOA - Externally Owned Account)**
A standard user account controlled by a private key (like MetaMask or Ledger).
*   *Vs. Smart Contract Account:* An EOA has a private key and can *initiate* transactions. A smart contract has code but no private key; it can only run when triggered by an EOA.

**Signatures (Digital Signatures)**
A mathematical proof that a message (like a transaction) was approved by the holder of a specific private key.
*   *Relevance:* In 1inch Fusion, users don't send a transaction to the blockchain directly. They "sign" an order message off-chain. Solvers take this signature and submit it to the blockchain. This is why Fusion swaps feel "instant" and "gasless" to the user until the fill happens.

## Infrastructure Layers

**Layer 1 (L1)**
The base blockchain (e.g., Ethereum Mainnet, Bitcoin, Solana). It handles security and consensus but is often slow and expensive.

**Layer 2 (L2)**
Scaling solutions (like Arbitrum, Optimism, Base) that sit "on top" of L1. They process transactions cheaply and fast, then "roll up" the data to L1 for security.
*   *Data Challenge:* 1inch supports many L2s. A data engineer must standardize data across these chains, even though they might have different block times or finality rules.

**EVM (Ethereum Virtual Machine)**
The "computer" that runs on every Ethereum node. It interprets the smart contract code (bytecode).
*   *EVM Compatibility:* Many other chains (Binance Smart Chain, Avalanche, Polygon) use the EVM, meaning 1inch can easily deploy its contracts there. This is why 1inch is "multichain."

**ERC-20**
The technical standard for fungible tokens (like USDT, UNI, 1INCH) on Ethereum.
*   *Key Data Field:* `decimals`. Not all tokens have 18 decimals (e.g., USDC has 6, WBTC has 8). Ignoring this field is the #1 error in crypto data analysis.



## Data Engineering Tools

**dbt (Data Build Tool)**
A transformation workflow tool that allows anyone who knows SQL to engineer data. It is the industry standard for transforming raw blockchain data (bronze) into usable analytics tables (gold) inside a warehouse.

**The Graph (Subgraphs)**
A decentralized indexing protocol for querying networks like Ethereum. It organizes blockchain data into efficient "Subgraphs" that can be queried with GraphQL, often serving as a middle layer between raw chain data and a frontend.
