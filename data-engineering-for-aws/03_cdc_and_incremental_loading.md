# Loading Methods: CDC vs Full vs Delta vs Snapshot

## Why this matters

Selecting the correct loading method per source is critical. Choosing wrong causes duplicates, missing deletes, high cost, or broken downstream semantics.

## Definitions (practical)

### Full load

Re-extract entire dataset each run.
- Use when: small tables, rare updates, simple sources, or initial loads.
- Risks: cost, long runtimes, pressure on source systems.

### Snapshot load

Extract a consistent “as of time T” view (may still be full size).
- Use when: you need point-in-time consistency across tables.
- Often paired with: partitioning by snapshot date.

### Delta (incremental) load

Extract only rows changed since a watermark (e.g., `updated_at`).
- Use when: source provides reliable monotonic timestamp/sequence.
- Risks: late arriving updates, clock skew, mutable timestamps.

### CDC (Change Data Capture)

Extract change events (insert/update/delete) with ordering metadata.
- Use when: you need deletes and precise change history.
- Risks: out-of-order events, replay/duplication, schema drift.

## Decision guide

Pick based on:
- **Change volume**: high change → CDC/delta; low change → snapshot/full.
- **Delete requirements**: need deletes → CDC (or special delete tracking).
- **Watermark reliability**: unreliable timestamps → CDC or full/snapshot.
- **Source constraints**: can’t handle heavy reads → CDC/delta preferred.

## Incremental correctness patterns

### Watermark strategy (delta)

- Use a watermark column with stable semantics (e.g., `last_modified_ts`).
- Use a **lookback window** to handle late arrivals (re-read last N minutes/hours/days).
- Deduplicate within the window using keys + highest `last_modified_ts`.

### CDC strategy

Store in Bronze:
- `op` (I/U/D)
- `op_ts` or `commit_ts`
- `sequence`/`lsn`/`event_id` if available

Dedup:
- Use `(primary_key, sequence)` or `(event_id)` as uniqueness.

Apply:
- Either append-only in Bronze (preferred) and compute latest in Silver,
- Or merge with careful “latest event wins” rules.

## Handling deletes

- CDC: represent deletes explicitly (`op = 'D'`) and propagate.
- Delta: if deletes aren’t represented, you need an auxiliary delete feed or periodic reconciliation snapshot.
- Snapshot: detect removals by comparing consecutive snapshots (expensive but possible).

## Checklists

- Load type chosen and justified for each source table.
- Watermark and lookback window defined (delta).
- Ordering/uniqueness keys defined (CDC).
- Dedup strategy defined and tested.
- Delete semantics defined and tested end-to-end.

## Q&A

1. When would you avoid delta loading even if the source has an `updated_at`?
2. What’s a lookback window and why is it needed?
3. How do you handle out-of-order CDC events?
4. How do you model deletes through Bronze → Silver?
5. How do you prove your incremental logic is correct?
