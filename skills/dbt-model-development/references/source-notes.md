# Source notes for version 0.1

This skill is a synthesis, not a copy of any one source. The sources below were reviewed on 26 September 2026.

## Primary sources used

- [dbt SQL models](https://docs.getdbt.com/docs/build/sql-models): model-as-select pattern, modularity, `ref()`, configuration, and dbt-managed transformations.
- [dbt data tests](https://docs.getdbt.com/docs/build/data-tests?name=Fusion&version=2.0): data tests as assertions about expected model data.
- [dbt continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration?version=2.0): targeted CI validation of modified assets and relevant downstream dependencies.
- [dbt model access](https://docs.getdbt.com/docs/mesh/govern/model-access): ownership and access boundaries for models used by others.
- [dbt project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies): published models as interfaces across projects.
- [dbt Labs: using dbt for analytics engineering](https://github.com/dbt-labs/dbt-agent-skills/blob/main/skills/dbt/skills/using-dbt-for-analytics-engineering/SKILL.md): inspect existing model metadata first, use `ref()`/`source()`, and treat consumer-facing breaking changes as migrations rather than silent edits.

## Community input considered

- [atlasfutures/dbt-skillz](https://github.com/atlasfutures/dbt-skillz) demonstrates a useful community pattern: giving an agent a current, generated model inventory and project context. This is not a universal modelling rule, so version 0.1 records it as a future enhancement rather than embedding its repository-specific content in the standard.

## Deliberate choices in this synthesis

- The skill requires an explicit grain, because it makes joins, keys, tests, and reviews materially more reliable.
- It does not prescribe layers, names, materialisations, or commands: the four teams have different project structures, and those belong in local repository guidance.
- It treats documentation and meaningful tests as part of model development, while allowing later dedicated skills to provide deeper procedures.
- Databricks operational safety remains a separate forthcoming skill so that it can be reviewed with platform owners rather than inferred from generic dbt guidance.
