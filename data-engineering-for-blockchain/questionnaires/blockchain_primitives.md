# Solutions: Subject 1 (EVM & Blockchain Primitives)

## 1. `msg.sender` vs. `tx.origin`
**Question:** Explain the difference between `msg.sender` and `tx.origin`. Why might a data engineer caring about "unique user count" prefer one over the other?

**Answer:**
*   **`msg.sender`:** The immediate address that called the function. This could be a user wallet (EOA) OR another smart contract.
*   **`tx.origin`:** The original EOA (wallet) that signed and initiated the entire transaction chain. It is *always* a user wallet, never a contract.
*   **Data Engineer's Choice:** For "Unique User Count," you typically want **`tx.origin`**.
    *   *Reason:* If a user interacts with 1inch through a smart wallet (like Argent or Gnosis Safe) or a proxy, `msg.sender` will be the contract address. Using `tx.origin` ensures you count the actual human/signer behind the trade, not just the proxy contract.

## 2. Internal Transactions (Traces)
**Question:** In the context of the EVM, what are "Internal Transactions" (or Traces)? Why do they not appear in standard `eth_getTransaction` queries?

**Answer:**
*   **Definition:** Value transfers (usually ETH) that occur *between contracts* during the execution of a main transaction. They are not separate transactions on the blockchain; they are "side effects" of code execution.
*   **Why they are hidden:** The blockchain only stores the *input* (the transaction call) and the *output* (the state change). It does not explicitly store every intermediate step in the block header to save space.
*   **Data Implication:** Standard RPC calls like `eth_getTransaction` only show the top-level move. To see that a user's swap internally routed ETH to 3 different pools, you must use a specialized node with **Tracing enabled** (e.g., `debug_traceTransaction`) to "replay" the code and capture these internal steps.

## 3. Event Log Structure
**Question:** What is the structure of an Event Log? Specifically, what is `Topic[0]` vs `Topic[1-3]` vs `Data`?

**Answer:**
*   **`Topic[0]`:** The Keccak-256 hash of the event signature (e.g., `Transfer(address,address,uint256)`). This tells you *what* kind of event it is.
*   **`Topic[1-3]`:** The **indexed** parameters. These are stored in a way that makes them efficiently searchable by Bloom filters.
    *   *Example:* In `Transfer(from, to, value)`, `from` and `to` are usually indexed (Topics 1 and 2) so you can quickly find "all transfers FROM Alice."
*   **`Data` (Non-Indexed):** The remaining parameters that are not indexed.
    *   *Example:* The `value` (amount) of tokens transferred is usually too large/random to be indexed, so it is stored in the hex-encoded `Data` field. You must decode this field to get the actual number.

## 4. Probabilistic Finality
**Question:** Why do we say Ethereum has "probabilistic finality"? How does this affect a data pipeline design compared to a traditional bank database?

**Answer:**
*   **Concept:** In traditional databases, once you `COMMIT`, the data is permanent. In blockchains (especially Proof-of-Work or certain L2s), a block is only "likely" to be permanent. A longer chain could theoretically be built that excludes your block (a "Reorg").
*   **"Probabilistic":** The more blocks are added *on top* of your block, the lower the probability it will be reverted. After ~12-15 minutes (on ETH Mainnet), it is considered "final."
*   **Pipeline Impact:**
    *   You cannot treat the most recent data as "Truth."
    *   **Hot/Cold Architecture:** You usually need a "Real-time" table (updates instantly, high risk of being wrong) and a "Finalized" table (lags by 15 mins, but is 100% accurate).
    *   **Reorg Handling:** Your ingestion script must constantly check: "Does the `parentHash` of the new block match the `hash` of the last block I saved?" If not, you must delete the last few rows and re-download.
