# Solutions: Subject 5 (Security & Anomaly Detection)

## 1. Gas Price Spikes (Congestion vs. Code)
**Question:** You notice a sudden spike in gas prices for transactions interacting with the 1inch Router. How do you investigate if this is a network congestion issue vs. an inefficient contract update?

**Answer:**
*   **The Investigation:**
    1.  **Baseline Comparison:** Compare the gas price of 1inch transactions against the global `base_fee_per_gas` of the network (Ethereum average).
        *   *Scenario A:* If Global Gas is 100 Gwei and 1inch Tx is 100 Gwei -> **Network Congestion** (External factor, e.g., an NFT mint).
        *   *Scenario B:* If Global Gas is 20 Gwei but 1inch Tx is costing 100 Gwei (or rather, consuming 5x more *gas units*) -> **Contract Inefficiency**.
    2.  **Gas Used vs. Gas Price:**
        *   Look at `gas_used` (units of computation), not just the cost in ETH. Did the *average gas units per swap* jump from 150k to 300k after a recent protocol upgrade? If yes, a bad code deployment caused it.

## 2. TVL Drops (Hack vs. Migration)
**Question:** A specific pool's TVL (Total Value Locked) drops by 99% in one block. Is this a hack or a migration? How do you verify this programmatically?

**Answer:**
*   **Programmatic Verification:**
    1.  **Check the `to` Address:**
        *   Look at the large `Transfer` event that drained the pool.
        *   *Migration:* The funds moved to a **New Contract** (e.g., Uniswap V2 -> V3). You can check if the destination address is a verified contract with similar bytecode or created by the protocol's "Factory."
        *   *Hack:* The funds moved to an **EOA (Wallet)** or a contract that immediately sent funds to Tornado Cash (Privacy Mixer).
    2.  **Check the Function Call:**
        *   *Migration:* Often uses a specific function like `migrate()` or `removeLiquidity()`.
        *   *Hack:* Often uses an obscure or unverified function, or a standard `swap()` that manipulated the price to drain value (Flash Loan attack).

## 3. Reentrancy Attacks
**Question:** What is a "Reentrancy Attack"? How might the data footprint of a reentrancy attack look different from a normal transaction in your logs? (Hint: Recursive calls).

**Answer:**
*   **Definition:** An attacker calls a function (e.g., `withdraw`), and before the contract updates the user's balance, the attacker's code calls `withdraw` again *recursively*, draining funds before the "balance = 0" update happens.
*   **Data Footprint:**
    *   **Deep Call Stack:** In a trace (internal transaction view), you will see the *same* function being called many times within the *same* transaction depth, often with the same parameters.
    *   **Repeated Events:** You might see 50+ `Transfer` (Out) events in a single transaction, all to the same recipient, without any corresponding `Deposit` events.
    *   **Gas Usage:** These transactions often consume nearly exactly the block gas limit (e.g., 30M gas) to maximize the drain in one go.

## 4. Flash Loan Attack Identification
**Question:** How would you programmatically distinguish a legitimate Flash Loan (arbitrage) from a Flash Loan Attack in your data pipeline?

**Answer:**
*   **The Shared Footprint:** Both start with a massive borrow from a lending pool (Aave/DyDx) and end with a repayment in the same transaction.
*   **The Differentiator:**
    *   *Legitimate Arbitrage:* The logic typically stays within standard "Swap" patterns. The profit is usually modest (0.1% - 5%).
    *   *Attack:* The logic often involves **Price Manipulation**.
*   **Detection Query Logic:**
    *   Look for transactions where:
        1.  `flash_loan_amount` > $1M USD.
        2.  Inside the trace, a specific Pool's `reserve` ratio changes by > 10% (huge slippage).
        3.  The transaction calls a non-standard function (not `swap`) or interacts with a governance contract.
        4.  **Key Metric:** "Profit to Gas Ratio." Attacks often pay massive bribes to miners (coinbase.transfer) to get included. If `miner_bribe` > 10 ETH, flag it.

## 5. "Fake Volume" via Self-Funding Cycles
**Question:** You are analyzing a new token listed on 1inch to determine if it's safe for users. You suspect "Sybil" wash trading. How do you use Graph Analysis to prove it?

**Answer:**
*   **The Concept:** Sophisticated wash traders use hundreds of different wallets to make it look like "organic" community interest.
*   **Graph Analysis Strategy:**
    1.  **Node = Wallet**, **Edge = Transfer**.
    2.  **The "Funding" Edge:** Look backwards at the *first* transaction of every wallet. Who sent them the ETH for gas?
    3.  **The Star Topology:** If 500 "independent" trading wallets all received their initial ETH from the *same* Distributor Wallet (or a CEX withdrawal with the same Tag), they are a single entity.
    4.  **The "Hot Potato" Loop:** Visualizing the token flow often reveals a closed loop: A -> B -> C -> D -> A. Real tokens disperse; wash traded tokens recirculate.

## 6. Front-Running vs. Back-Running Data Signatures
**Question:** How do you differentiate a "Front-Running" bot from a "Back-Running" bot in block data? Why does 1inch care?

**Answer:**
*   **Context:**
    *   *Front-Run:* Buying *before* a victim to raise the price. (Predatory).
    *   *Back-Run:* Buying *after* a victim creates a price imbalance to correct it. (Arbitrage/Benign).
*   **Data Identification:**
    *   You must sort the transactions in the block by `transaction_index`.
    *   **Pattern 1 (Predatory):** Bot Tx (Index 3) -> Victim Tx (Index 4). *Bot pays higher gas price.*
    *   **Pattern 2 (Benign Arbie):** Victim Tx (Index 4) -> Bot Tx (Index 5). *Bot pays lower gas price but likely pays a direct bribe to the builder.*
*   **Why 1inch Cares:** 1inch Solvers often *Back-Run* their own fills to rebalance their inventory. This is healthy. Front-running user swaps is "Toxic MEV" that 1inch Fusion explicitly bans.

## 7. Detecting "Approve" Phishing Scams
**Question:** A user complains their wallet was drained after interacting with a fake 1inch site. How do you find the malicious contract using data logs?

**Answer:**
*   **The Mechanism:** The user didn't swap; they signed an `Approve(Spender, Amount)` transaction.
*   **Investigation Steps:**
    1.  Filter `Approval` events (ERC-20 standard) where `owner` = User Address.
    2.  Check the `spender` address.
    3.  **The Anomaly:** Compare the `spender` against the official list of 1inch Router contracts.
        *   If `spender` is NOT in the [1inch Official Contract Registry] AND `value` is Infinite (2^256 - 1), it is likely a phishing contract.
    4.  **Trace Funds:** Follow the subsequent `TransferFrom` call made by that Spender to see where the funds went.

## 8. Oracle Manipulation Detection
**Question:** How do you detect if a pool's price is being manipulated via Oracle attacks?

**Answer:**
*   **Concept:** Attackers manipulate the spot price of a pool (e.g., Uniswap) that another protocol uses as an Oracle.
*   **Data Check:**
    *   Compare the **Spot Price** (Reserve A / Reserve B) vs. the **Time-Weighted Average Price (TWAP)**.
    *   **Alert Rule:** If `abs(Spot_Price - TWAP) > 20%` in a single block, trigger an alert.
    *   *Why:* Real market moves take time (moving the TWAP slowly). Flash loan attacks move the Spot Price instantly but leave the TWAP mostly unchanged (unless they sustain the attack for many blocks).
