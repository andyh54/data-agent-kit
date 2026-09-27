# Databricks SQL asset classification

## Choose the right destination

| Existing asset | Default destination | Important decision |
| --- | --- | --- |
| `CREATE VIEW AS SELECT` | dbt view model | Preserve output schema, ownership/consumer expectations, and source lineage. |
| CTAS or transformed managed table | dbt table model | Put storage/materialisation configuration in dbt; retain only approved Databricks physical settings. |
| `MERGE` maintaining one analytical dataset | dbt incremental model, if its correctness design is explicit | Define unique key, source change signal, late data, deletes, schema change and rebuild path. |
| Temporary view/table inside a transformation script | CTE or intermediate model | Keep the logical step; do not recreate disposable operational objects by default. |
| SQL UDF implementing reusable data expression | Keep as a governed Unity Catalog function or replace with a dbt macro only after deciding whether it must exist at query runtime | A dbt macro is compile-time templating; it is not automatically equivalent to a callable database function. |
| Procedure that returns a declarative result set | One or more dbt models | Separate transformation logic from invocation/parameter handling. |
| Procedure/script with control flow, cursors, calls, exception handlers, DDL/DCL, maintenance, or notifications | Orchestration or managed Databricks routine, pending owner decision | Do not force procedural runtime behaviour into a dbt model. |
| Multi-table transaction | Platform/transaction design review | Retain only with explicit Databricks support, ownership and rollback requirements. |

## Security and governance

Unity Catalog procedures can run with invoker or definer security. Converting a procedure to a dbt model can therefore change who reads or writes data and which grants are relied upon. Record the source owner, execution security mode, required privileges, target owner, and consumer grants before cutover.

Published views/tables should be treated as interfaces. If the dbt conversion changes object name, location, schema, data type, semantics, freshness, or access, plan a compatible migration with affected consumers.

## Databricks-specific review prompts

- Is the original table/view managed or external, and are its physical location/properties intentionally retained?
- Does the asset rely on `OPTIMIZE`, `VACUUM`, caching, clustering, or maintenance that belongs in a platform workflow rather than model SQL?
- Does a procedure’s `SQL SECURITY INVOKER` or `SQL SECURITY DEFINER` behaviour create an access boundary that must remain?
- Are dynamic identifiers, session variables, parameter markers, or `EXECUTE IMMEDIATE` doing work that cannot safely become static dbt SQL?
- Does the source use a multi-statement transaction whose atomicity cannot be replaced by independently built dbt models?

## Source basis

- Databricks SQL scripting supports procedural blocks, DDL/DCL/DML, variables, flow control, and stored procedures; these capabilities are not automatically dbt transformations: [Databricks SQL scripting](https://docs.databricks.com/gcp/en/sql/language-manual/sql-ref-scripting).
- Stored procedures are Unity Catalog assets with defined parameters and security/data-access metadata: [DESCRIBE PROCEDURE](https://docs.databricks.com/gcp/en/sql/language-manual/sql-ref-syntax-aux-describe-procedure).
- Databricks distinguishes invoker and definer security, which can change authorisation during a conversion: [authorised versus session user](https://docs.databricks.com/gcp/en/sql/language-manual/sql-ref-authorized-user).
- Transactions have distinct support and operational requirements: [Databricks transactions](https://docs.databricks.com/aws/en/transactions).
