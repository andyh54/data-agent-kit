---
name: dbt-databricks-sql-conversion
description: Convert existing Databricks SQL tables, views, SQL scripts, and stored procedures into appropriate dbt models or explicitly retain non-dbt platform behaviour. Use when modernising legacy Databricks SQL assets into a governed dbt project.
---

# Databricks SQL to dbt conversion

Use this skill with `dbt-model-development`. Read the [legacy SQL conversion method](../dbt-model-development/references/legacy-sql-conversion-method.md) and [Databricks SQL asset classification](references/databricks-sql-asset-classification.md) before proposing changes.

## Required approach

1. Inspect the complete Databricks asset, its invocation path, Unity Catalog object identity, owner/security mode, inputs, outputs, and side effects. For a stored procedure, inspect the full `BEGIN ... END` body and any called routines.
2. Classify the asset before translating it. A view/table transformation is often suitable for dbt; procedural orchestration, security boundaries, maintenance, and multi-object transactions may not be.
3. Write a conversion brief and target DAG. Preserve the logical data product and consumer interface, not the legacy DDL/DML sequence.
4. Convert suitable transformations into dbt models using `source()`/`ref()`, explicit model configuration, documentation, tests, and a materialisation decision. Use the repository's approved Databricks safety patterns.
5. Reconcile old and new outputs over an agreed period before consumer cutover. Treat changed Unity Catalog ownership, grants, procedure security, or transactions as a human/platform decision.

## Escalate before converting

Do not automatically convert SQL procedures/scripts that use `SQL SECURITY DEFINER`, dynamic SQL, `GRANT`/`REVOKE`, `OPTIMIZE`/`VACUUM`, `CALL`, loops/cursors, error handlers, multi-table `BEGIN ATOMIC` transactions, or external side effects. Determine whether the behaviour remains a managed Databricks routine, moves to orchestration, or needs redesign.

## Conversion output

Return a concise conversion pack: asset classification, source-to-target mapping, target model grain/interfaces, proposed DAG, materialisation and Unity Catalog implications, tests/reconciliation, open questions, and cutover risks.
