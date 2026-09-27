# Incremental model standard

## Choose incremental deliberately

Incremental processing is appropriate only when the model can be updated correctly without rebuilding all historical rows and when that trade-off is worth its operational complexity. A full-table model can be safer and clearer for modest data volumes or volatile logic.

Before adopting or changing incremental logic, state:

- why incremental processing is needed;
- the update strategy and the record key, where the strategy requires one;
- the watermark or change-detection rule;
- handling for updates, deletes, duplicates, and late-arriving records;
- the lookback window, if used; and
- how a full rebuild or backfill would be performed safely.

## Correctness requirements

- The incremental predicate must be compatible with the model grain and update strategy.
- Re-running the same eligible input should not create duplicates or corrupt existing rows.
- A late record or correction must either be captured by the design or be an explicitly accepted limitation with an operational remedy.
- Do not use the current timestamp as a substitute for a source change signal unless the repository has an approved pattern that makes it safe.

## Review and validation

Compare the incremental result with an appropriate known slice or full-build expectation when it is safe and practical. Include tests or checks for the key and the failure mode the incremental design is intended to prevent. Any full refresh or backfill must follow the repository's Databricks safety and deployment controls.
