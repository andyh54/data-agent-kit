# Legacy SQL conversion method

Use this method when translating a SQL asset into dbt. The goal is to preserve the agreed business outcome while redesigning the implementation for a declarative dbt project—not to produce a line-for-line syntax conversion.

## 1. Inventory and classify the asset

Record its owner, invocation/schedule, inputs, outputs, consumers, side effects, parameters, and any dependent procedures/functions. Then classify it:

| Source asset behaviour | Normal dbt destination |
| --- | --- |
| Single declarative `SELECT`, view, or CTAS transformation | One dbt model, in the appropriate layer |
| Source cleanup/type/name standardisation | Source declaration plus staging model |
| Reusable join, deduplication, bridge, or business preparation | Intermediate model(s) |
| Consumer-facing entity, fact, dimension, or report dataset | Mart model |
| Rebuild/append/upsert of one derived dataset | Table or incremental model, after incremental design review |
| Historical change tracking | Snapshot or explicitly modelled history, after agreeing the history semantics |
| Scheduling, loops, multi-object workflow, retries, grants, maintenance, or notifications | Orchestration/platform process; do not force into a dbt model |
| Security-definer logic, dynamic SQL, cross-system calls, or multi-table transaction | Human/platform review before any conversion decision |

## 2. Write a conversion brief before coding

For each target model, state:

- business purpose and one-row grain;
- source asset and all logical inputs;
- output schema/consumer interface and key expectation;
- filters, joins, aggregation, deduplication, and business rules to preserve;
- materialisation and, if applicable, incremental key/strategy/late-data behaviour;
- behaviour intentionally not converted to dbt; and
- reconciliation checks and the approved cutover plan.

If the legacy asset’s purpose or output is unclear, stop and ask. Do not infer a business definition from procedural SQL alone.

## 3. Redesign, then translate

- Replace hard-coded relation names with declared `source()` and `ref()` dependencies.
- Split temporary steps into CTEs only when they aid readability, or intermediate models when the logic is reusable, independently testable, or too complex for one model.
- Convert persistent table/view creation into dbt configuration and a `select` statement; do not put routine DDL/DML inside a dbt transformation model.
- Treat an upsert as a correctness design problem, not merely a `MERGE` translation. Define the key, update/change detection, late data, deletes, backfill and rebuild path.
- Preserve the outcome of business rules, not incidental legacy implementation order.

## 4. Reconcile before cutover

Use a bounded, repeatable comparison against the approved legacy output. Select checks appropriate to the model:

- row counts by date/domain/status;
- key-level anti-joins or set differences;
- null, duplicate, and relationship checks;
- aggregate financial or operational totals;
- representative edge cases: nulls, dates/time zones, duplicate inputs, corrections, late records, and boundary values.

Record the comparison scope, data cut-off, known accepted differences, results, and unresolved variance. Do not cut over consumers based solely on a successful compile.

## 5. Release safely

Keep the legacy asset and new model in parallel only for an agreed, time-bounded reconciliation period. Make a consumer migration plan for interface changes, obtain owner approval, and retire the legacy asset through the platform’s normal change controls.
