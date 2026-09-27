---
name: dbt-sql-server-conversion
description: Convert SQL Server T-SQL views, tables, stored procedures, and scheduled transformations into well-designed dbt models for Databricks. Use when analysing or migrating legacy SQL Server transformation logic; do not use for a simple new dbt model with no legacy asset.
---

# SQL Server to dbt conversion

Use this skill with `dbt-model-development`. First read the [legacy SQL conversion method](../dbt-model-development/references/legacy-sql-conversion-method.md), then read [SQL Server to Databricks considerations](references/sql-server-to-databricks-considerations.md).

## Required approach

1. Read the entire source asset and its directly called procedures/functions before proposing a conversion. Capture parameters, temp/table variables, DML, transactions, error handling, output/result sets, and side effects.
2. Classify each logical step as a dbt model, macro, test, seed, snapshot, or non-dbt platform/orchestration responsibility. A stored procedure is not automatically one dbt model.
3. Produce a conversion brief and proposed target DAG before changing files. Name unresolved semantics explicitly.
4. Implement set-based, declarative transformations using `source()`/`ref()` and the target repository's layer/naming conventions. Do not copy `CREATE`, `DROP`, `INSERT`, `UPDATE`, `DELETE`, `MERGE`, `EXEC`, or control-flow statements into a dbt model.
5. Add data tests, unit tests where business logic is complex, documentation, and a source-to-target reconciliation plan. Report evidence and accepted differences in the PR.

## Escalate before converting

Ask for platform/owner direction when the source asset contains multi-table transaction boundaries, dynamic SQL/object names, security/permission changes, external calls, operational notifications, non-idempotent side effects, or unclear delete/history semantics.

## Conversion output

Return a concise conversion pack: asset classification, source-to-target mapping, target model grain/interfaces, proposed DAG, materialisation decision, tests/reconciliation, open questions, and cutover risks.
