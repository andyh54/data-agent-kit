---
name: dbt-model-development
description: Plan, create, refactor, or materially change a dbt SQL model. Use when work affects a model's SQL, configuration, grain, dependencies, schema, or consumer-facing behaviour.
---

# dbt model development

Use this skill together with the active repository's instructions. Repository-specific rules take precedence, including its project layout, naming conventions, supported dbt commands, development target, and ownership model.

Read [the shared development standard](references/dbt-development-standard.md) for every model change. Read the additional reference that matches the change:

- [Modelling principles](references/modelling-principles.md) for a new model, a changed grain, a new join, or a material refactor.
- [Incremental model standard](references/incremental-model-standard.md) for incremental logic, late-arriving data, backfills, or materialisation changes.
- [Validation and change scope](references/validation-and-change-scope.md) before testing or reporting completion.

The [version 0.1 candidate modelling and development standards](references/candidate-modelling-and-development-standards.md) provide additional review guidance, including dbt Project Evaluator-inspired design signals. Use them as decision support, not as automatic pass/fail checks.

## Workflow

1. **Orient before editing.** Read repository instructions, the target model SQL and YAML, its upstream sources/models, and relevant downstream consumers. Establish the requested outcome and any ambiguity.
2. **Identify the interface impact.** Treat a model name, grain, column name, data type, business meaning, or access-level change as potentially breaking when another model, dashboard, or team may consume it. Do not make a breaking change silently; propose a migration or versioning approach and seek confirmation.
3. **Make the design explicit.** State the model's purpose, grain, key(s), input dependencies, major joins/filters, expected handling of duplicates and late data, materialisation, and risks. Ask a focused question when any of these cannot be determined from the request and repository context.
4. **Implement in dbt's dependency graph.** Use `source()` for declared raw sources and `ref()` for dbt models. Preserve the repository's conventions; use clear, staged SQL and explicit output columns unless a deliberate local convention says otherwise.
5. **Update the model as a data product.** Add or revise model and column descriptions, and propose meaningful data tests for the important business rules and failure modes. Do not add superficial tests merely to increase a count.
6. **Validate proportionately and report evidence.** Run the repository-approved, targeted checks that are safe for the environment. Report what ran, what passed or failed, what was not run, and any remaining assumptions or downstream risks. Never claim a check ran when it did not.

## Boundaries

- Do not hard-code warehouse relation names where a declared `source()` or `ref()` applies.
- Do not run production, destructive, full-refresh, or broad backfill operations unless the repository workflow explicitly authorises them. Flag the need instead.
- Use locally installed dbt Labs skills when available for their specialised procedures, such as documentation, unit tests, and model analysis. This skill supplies the shared decision standard; it does not replace those procedures.
