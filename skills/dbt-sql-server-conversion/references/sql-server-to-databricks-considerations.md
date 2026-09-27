# SQL Server T-SQL to dbt/Databricks considerations

## Procedural constructs are design signals

| T-SQL construct | Default conversion approach |
| --- | --- |
| `#temp` table or table variable used as one query stage | A named CTE, if it is local to one readable transformation |
| `#temp` table reused, independently reconciled, or too complex for one query | An intermediate dbt model, not a long-lived scratch table |
| `SELECT INTO`, `CREATE TABLE AS`, or create/drop/rebuild routine | A dbt model with deliberate materialisation/configuration |
| `INSERT`/`UPDATE`/`DELETE`/`MERGE` that maintains one derived output | Consider an incremental model only after defining keys, updates, deletes, late data and rebuild behaviour |
| Stored-procedure parameters | Determine whether they are business inputs, environment configuration, run controls, or an API contract. Do not silently replace a runtime parameter with a hard-coded dbt value. |
| `IF`, `WHILE`, cursor, `TRY/CATCH`, nested `EXEC`, transaction, email/notification | First attempt a set-based transformation for data logic; otherwise retain/rebuild it as orchestration or platform logic with owner review |
| Dynamic SQL | Do not mechanically convert. Establish the controlled set of allowed objects/behaviour and get security review if identifiers or predicates are dynamic. |

SQL Server temporary tables have session/procedure scope; their lifetime and side effects should not be carried into dbt by creating persistent scratch tables. The target design should retain only the logical transformation they enabled.

## Semantics to verify explicitly

Do not assume T-SQL and Databricks SQL behave identically. Include evidence for the cases that affect the asset:

- data-type range, precision/scale, rounding, and integer division;
- `NULL` handling, string concatenation, case/collation and trailing spaces;
- date/time zone handling, week/year boundaries, and date arithmetic;
- `TOP`/ordering semantics and deterministic row selection;
- identity/surrogate-key generation and duplicate source records;
- `MERGE` match conditions, delete behaviour, and concurrency assumptions; and
- error/transaction behaviour and any output/audit tables.

Prefer explicit casts, explicit null handling, deterministic ordering, and tests for the business outcome over dialect-specific shorthand.

## Testing and reconciliation pattern

For each converted output, identify its natural key and compare a representative agreed data window. Reconcile row counts and business totals by meaningful dimensions, then investigate key-level set differences. Include source records that exercise nulls, ties, updates, late data, deleted/cancelled records, and decimal/date boundaries.

Use dbt data tests to protect the model’s declared grain and business invariants. Use unit tests for translated branching, complex calculations, or incremental mode. The dbt framework’s materialisation implementation itself is not proven by a SQL comparison, so validate the intended incremental inputs and perform a controlled end-to-end run where permitted.

## Source basis

- SQL Server temporary-table scope and behaviour: [Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/create-table-transact-sql?view=sql-server-ver17).
- SQL Server `MERGE` performs target insert/update/delete actions and requires its matching semantics to be understood: [Microsoft Learn](https://learn.microsoft.com/en-us/sql/t-sql/statements/merge-transact-sql?view=sql-server-ver17).
- dbt incremental models require an explicit filter and, where updates occur, a key; the model SQL must work in both full and incremental modes: [dbt documentation](https://docs.getdbt.com/docs/build/incremental-models).
- dbt unit-test guidance for incremental logic: [dbt documentation](https://docs.getdbt.com/docs/build/unit-tests?name=Fusion&version=2.0).
