# Solutions: Subject 4 (SQL & Analytics Logic)

## 1. Calculating Volume for Tokens with No Price Feed
**Question:** Write the logic to calculate "Volume in USD" for a token that has no direct USD price feed on-chain (e.g., a random shitcoin). How do you derive its price?

**Answer:**
*   **The Logic: "Path of Liquidity" (Transitive Pricing).**
    1.  **Identify the Trade:** User swapped `Shitcoin` -> `ETH`.
    2.  **Find the Anchor:** ETH has a known, reliable USD price (e.g., from Chainlink or CEX API).
    3.  **Derive:**
        *   If 1,000,000 `Shitcoin` = 1 `ETH`.
        *   And 1 `ETH` = $2,000.
        *   Then Price of `Shitcoin` = $0.002.
        *   Volume USD = 1,000,000 * $0.002 = $2,000.
*   **SQL Implementation:** You need a `prices` table (hourly/daily) for major "Anchor Tokens" (ETH, USDC, WBTC). When calculating volume, you join the trade on the *output* token if the input token is unknown (or vice versa).
    *   *Edge Case:* If the trade is `Shitcoin A` -> `Shitcoin B`, you look for a "Router Path" event or assume the trade routed through a stablecoin internally, requiring a Join on the trace/internal tx table.

## 2. The "Path of a Swap" (Linking Events)
**Question:** Explain the "Path of a Swap." If a user swaps USDC -> DAI -> ETH, how many `Transfer` events are emitted? How do you link them all to one "Trade" in your analytics table?

**Answer:**
*   **Event Sequence:**
    1.  `Transfer` (User -> Pool A): USDC out.
    2.  `Transfer` (Pool A -> Router/Pool B): DAI out.
    3.  `Transfer` (Pool B -> User): ETH in.
    *   *Total:* At least 3 Transfer events.
*   **Linking (The "Transaction Hash" Grouping):**
    *   All these events share the **same `transaction_hash`**.
    *   **Logic:**
        *   **Input:** Find the Transfer where `from` = `tx.origin` (User).
        *   **Output:** Find the Transfer where `to` = `tx.origin` (User).
        *   *Filtering:* Ignore the intermediate "hop" transfers (Pool -> Pool) for the high-level summary.
        *   *1inch Complexity:* 1inch routers often "unrwap" WETH to ETH at the end. This appears as a `Withdrawal` event, not a Transfer. Your SQL must handle this case.

## 3. Detecting Wash Trading
**Question:** How do you detect "Wash Trading" in a dataset of DEX volume? What patterns would you look for in `sender` and `recipient` addresses?

**Answer:**
*   **Definition:** Fake volume created to manipulate metrics or farm rewards.
*   **SQL Pattern Matching:**
    1.  **A -> B -> A Loop:** Look for addresses that buy and sell the same token repeatedly within a short window (e.g., < 1 hour) with near-zero net profit (just losing gas fees).
        *   `SELECT address, count(*) FROM trades GROUP BY address HAVING count(*) > 50 AND sum(abs(net_profit)) < 10`
    2.  **Self-Funding Networks:**
        *   Address X funds Address Y and Z.
        *   Y and Z trade with each other.
        *   *Detection:* Trace the source of ETH for the gas fees. If 100 "different" traders all received their initial gas from the same wallet, it's a Sybil/Wash farm.
    3.  **Zero-Slippage Consistency:** Normal trades have random slippage. Wash trades (if they control the liquidity pool) might have perfectly matched, non-random execution prices.
