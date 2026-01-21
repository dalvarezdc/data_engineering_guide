# Solutions: Subject 3 (Data Engineering & Architecture)

## 1. Handling `value` Precision (Uint256)
**Question:** You are designing a table to store Token Transfers. How do you handle the `value` column? (Hint: Precision).

**Answer:**
*   **The Problem:** The EVM uses `uint256` (up to 78 digits). Standard SQL types (`BIGINT`, `FLOAT`) max out at 64 bits (~19 digits) or lose precision.
*   **The Solution:**
    1.  **NUMERIC / DECIMAL:** In warehouses like BigQuery/Postgres, use `NUMERIC` (usually supports 38+ digits). This is best for calculations but can be slow.
    2.  **STRING (VARCHAR):** The safest, most "lossless" format. Store the raw number as a string (e.g., `"1000000000000000000"`).
        *   *Pros:* Zero precision loss, compatible with everything.
        *   *Cons:* Cannot sum/avg directly without casting.
    3.  **Split Columns (Advanced):** Store two columns: `value_raw_string` (for record keeping) and `value_float` (divided by decimals, for quick-and-dirty analytics).
*   *1inch Context:* NEVER use `FLOAT` for financial reporting. Losing 0.00000001 BTC is acceptable; losing 1 wei in a smart contract accounting audit is a failure.

## 2. Handling Chain Reorgs
**Question:** Your real-time pipeline just ingested a block that was later "reorged" (orphaned). Describe the step-by-step mechanism your system uses to fix the database.

**Answer:**
*   **Mechanism: "Look-Back Validation"**
    1.  **Ingestion:** When importing Block `N`, always read the `parentHash` field.
    2.  **Validation:** Compare this `parentHash` with the `block_hash` of Block `N-1` currently stored in your database.
    3.  **Detection:**
        *   If `Match`: Continue.
        *   If `Mismatch`: A reorg has occurred. The chain has switched to a different fork.
    4.  **Remediation (The "Rollback"):**
        *   Recursively check `N-2`, `N-3`, etc., until you find a common ancestor (a matching hash).
        *   **DELETE** all rows in your database where `block_number > common_ancestor_height`.
        *   **RE-IMPORT** the new blocks starting from that height.
*   *Architecture:* This is why many pipelines have a `status` column: `PENDING` (recent blocks) vs. `FINALIZED` (older blocks).

## 3. Efficient Batch Querying (Multicall)
**Question:** You need to query the balance of 10,000 users at a specific block height. `eth_getBalance` is too slow. What is a more efficient approach?

**Answer:**
*   **The Problem:** Sending 10,000 individual HTTP requests to an RPC node is slow (network latency) and expensive (rate limits).
*   **The Solution: Multicall Smart Contract.**
    *   Use a "Multicall" contract (deployed on almost every chain). It has a function that accepts an *array* of calls (target, data).
    *   You pack 1,000 `balanceOf` calls into a **single** RPC request to the Multicall contract.
    *   The contract executes them all in one go and returns an array of results.
    *   *Result:* You reduce 10,000 requests to ~10 requests.
*   *Alternative:* Some premium RPCs (like Alchemy) offer specialized APIs (`alchemy_getTokenBalances`), but Multicall is the universal, chain-agnostic engineering solution.

## 4. Deduplication
**Question:** How would you deduplicate transactions if you are ingesting from multiple RPC providers simultaneously to ensure uptime?

**Answer:**
*   **Strategy: "Upsert" (Update/Insert) on Primary Key.**
    *   **Primary Key:** In blockchain data, the `transaction_hash` is globally unique.
    *   **Logic:**
        *   Worker A (Infura) fetches Tx `0x123`.
        *   Worker B (Alchemy) fetches Tx `0x123`.
        *   Database writes use an `ON CONFLICT DO NOTHING` (Postgres) or `MERGE` (Snowflake) clause based on the `transaction_hash`.
    *   *Advanced:* If you are collecting Mempool data (where timestamps vary), you might keep the *first* one seen (lowest latency) and discard the second.
    *   *Partitioning:* Ensure your database partitions by `block_date` or `block_number` so deduplication checks don't scan the entire petabyte history.
