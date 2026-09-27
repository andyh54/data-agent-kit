# dbt development standard

## Purpose

Every dbt model change should be understandable, traceable through dbt's dependency graph, safe for its consumers, and supported by evidence. This is a cross-team baseline, not a replacement for a repository's own modelling conventions.

## Before changing a model

- Read the model SQL, its YAML properties, and the repository instructions.
- Identify upstream sources and models, downstream consumers, owner, and current materialisation/configuration where the repository exposes them.
- Decide whether the request is a correction, enhancement, refactor, performance change, or interface change.
- Confirm the intended data grain: what one row represents. State the key or combination of fields expected to identify a row.

## Build in the dependency graph

- Use `source()` for a declared source and `ref()` for a dbt model. Do not replace these with hard-coded database, schema, or table names.
- Keep transformations modular and readable. Use named, purposeful stages where they help explain source preparation, business logic, or final presentation.
- Select the output columns deliberately. Avoid `select *` in durable models unless the repository explicitly treats the model as a controlled pass-through and manages the schema consequences.
- Keep joins, filters, deduplication, and type conversions visible enough for a reviewer to verify the stated grain.
- Put configuration where the repository convention expects it. Choose a materialisation because it fits use, cost, freshness, and update behaviour—not by habit.

## Treat models as interfaces

A model may already be an interface for dashboards, metrics, other projects, or other teams. Renaming or removing a model or column, changing a data type, changing the grain, or changing a column's business meaning may break a consumer.

For a possible breaking change, identify consumers and propose the safe path: a compatible transition, a new/versioned model, a coordinated migration, or explicit owner approval. Do not hide an interface change inside a refactor.

## Documentation and tests

- Describe the model's purpose and its important columns in the project's YAML/documentation convention.
- Add or update tests that protect meaningful expectations: required identifiers, uniqueness where grain requires it, permitted values, referential relationships, important business rules, and known edge cases.
- Explain why a normally expected test is not appropriate when one is intentionally omitted.

## Completion evidence

Record the change made, validation run, validation result, checks not run, assumptions, and any residual risk. The PR should allow a reviewer to understand both the intended outcome and the evidence for it.
