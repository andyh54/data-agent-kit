# Candidate modelling and development standards

**Status:** version 0.1 candidate guidance. It supports development and assessment conversations; teams should confirm or adapt it through the repository assessment before treating a rule as mandatory.

This is the first output of the standards method: it converts researched practices into statements that the four teams can test against their existing projects. A rule becomes active only after the assessment worksheet is completed and the agreed wording is moved into the relevant skill reference.

The dbt Project Evaluator-inspired entries below are **review signals, not pass/fail checks**. They prompt an engineer, agent, or reviewer to examine a design choice and record a rationale where necessary. They are not a target to maximise, nor a reason to refactor working models without a clear benefit.

## 1. Model purpose, grain, and interface

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-01 | Every persisted, consumer-relevant model must state its purpose and grain: what one row represents and what makes it unique. | Grain makes joins, testing, reconciliation, and safe downstream use reviewable. | Simple source-aligned staging models may use a concise grain statement. |
| MD-02 | Every model whose output has an identifier must have its key expectation documented and tested where the expectation is enforceable. | A stated key protects the model’s declared grain. | A relationship table may intentionally allow duplicate values in an individual column; test the composite key or document why uniqueness cannot be asserted. |
| MD-03 | A published/consumer-facing model is an interface. Renaming/removing a column, changing its type, meaning, or grain requires an explicit impact and compatibility assessment. | A successful dbt build does not prove dashboards, other projects, or external consumers remain correct. | Internal, private models can use a lighter assessment, subject to repository ownership rules. |

## 2. Layering and ownership of logic

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-04 | A staging model standardises one declared source: naming, types, basic cleanup, source-level filtering, and source-record identifiers. It does not normally join unrelated business entities or define reusable business metrics. | Source-facing change is isolated once, preventing drift and repeated cleanup downstream. | A source that is already a governed, conformed data product may be treated according to the repository’s documented exception. |
| MD-05 | Intermediate models hold reusable transformations: complex joins, entity resolution, deduplication, bridges, and business-rule preparation. A rule should have one clear owning model rather than being reimplemented across marts. | It makes business logic discoverable, testable, and reusable. | Small, one-use transformations may stay in the final model where a separate model would obscure rather than clarify. |
| MD-06 | Marts present stable, consumer-oriented entities, facts, dimensions, or agreed reporting outputs. They should not silently reimplement source cleanup or hide large, unrelated business transformations. | It gives consumers an understandable, stable layer and makes the lineage of logic legible. | Existing project architecture may use different layer names; the responsibilities, not the names, are the standard. |
| MD-07 | Cross-layer dependencies should normally move forward from source/staging through intermediate to mart. A backwards dependency or direct layer skip needs a documented reason. | This protects a directional, comprehensible DAG. | A deliberately simple model may safely reference staging directly; record the local convention rather than inventing needless intermediate layers. |

## 3. SQL and CTE structure

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-08 | Use `source()` for declared raw sources and `ref()` for dbt models; do not hard-code managed relation names. | dbt can then manage environment resolution, lineage, selection, and refactoring. | A repository may document approved exceptions for external relations. |
| MD-09 | SQL should be organised into named stages when that makes its transformation logic easier to inspect. Prefer a consistent flow—inputs, rename/cast/clean, transform/enrich, final output—over a single opaque statement. | Reviewers can verify grain, joins, filters, and business rules. | Do not create ceremonial CTEs for a trivial query; compact, clear SQL remains preferable. |
| MD-10 | Durable models should select output columns intentionally. `select *` is permitted only for a documented controlled pass-through or generated pattern with known schema-evolution handling. | Unexpected upstream schema changes should not silently become a consumer-facing interface change. | Use project-specific macros or patterns where they provide the same explicit control. |
| MD-11 | Each material join must preserve or deliberately change the stated grain. The model should make the relationship, unmatched-record treatment, and duplicate handling reviewable. | Join fan-out is a common way to create plausible but incorrect data. | Straightforward, well-established dimension lookups may need only concise comments/documentation. |

## 4. Keys and materialisation

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-12 | Where a model needs a stable row identifier and no suitable natural key exists, create a deterministic surrogate key from the fields that define the grain. | Provides a durable key for tests, joins, incremental processing, and downstream use. | Do not create a surrogate key merely as a convention when the model has no row-level identity requirement. Confirm the agreed macro, null-handling, and type convention for each repository. |
| MD-13 | Materialisation is a design choice based on consumer use, volume, refresh pattern, cost, and correctness. The rationale must be documented for non-default or high-cost choices. | A universal “tables everywhere” or “views everywhere” rule is rarely sound on Databricks. | Each repository may define defaults by layer; the shared rule is to make deviations and risks explicit. |
| MD-14 | An incremental model must document its key/strategy, change-detection rule, late-data treatment, and safe rebuild/backfill path. | Incremental logic can silently lose corrections or duplicate records without these decisions. | None for a material incremental change; lack of information is a question to resolve, not an assumption. |

## 5. Documentation, tests, and review evidence

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-15 | A new or materially changed consumer-relevant model is incomplete without model documentation, relevant column documentation, and meaningful tests. | Documentation gives people and agents semantic context; tests make key expectations executable. | A reasoned exception is recorded in the PR, for example an upstream limitation that prevents a reliable assertion. |
| MD-16 | Tests should reflect failure modes, not merely a generic checklist: grain/key, accepted values, relationships, business rules, late data, or source assumptions as relevant. | Quality improves when the test is linked to the actual risk introduced by the change. | Existing model debt can be improved progressively; the change should not claim the model is fully protected when it is not. |
| MD-17 | A PR records the actual outcome, data-grain/interface impact, validation run, results, checks not run, and residual risks. | Reviewers need evidence from the completed change, not a hypothetical pre-change plan. | Small, low-risk changes can use a shorter template, but should still state validation. |

## 6. DAG health, stewardship, performance, and governance

These candidates adapt the ideas in dbt Project Evaluator. Counts and graph patterns are diagnostic signals: their context matters more than a universal threshold.

| ID | Candidate rule | Why it matters | Acceptable exception / decision needed |
| --- | --- | --- | --- |
| MD-18 | Maintain an accurate source catalogue: normally one dbt source definition per physical source relation; describe and own sources that are in use; remove or explain unused definitions. | Accurate declarations give trustworthy lineage, documentation, freshness configuration, and change impact. | One physical relation may need more than one declared view only when the semantic use genuinely differs and the rationale is documented. |
| MD-19 | A model’s name, directory/layer, and dependencies should agree. A staging-labelled model normally depends on its declared raw source, rather than another staging, intermediate, or mart model. | A model’s role is visible in both the project tree and the DAG, preventing misleading structure. | Repositories may use different layer names or deliberate base/union patterns; document their equivalent responsibilities. |
| MD-20 | Treat complex DAG shapes as a design-review prompt: a source/model with high fan-out, a model with many joins, or a concept rejoined immediately downstream should trigger a check for duplicated logic, an unclear consumer boundary, or needless fragmentation. | Such patterns can reveal drift, join risk, excessive model complexity, or logic that belongs in a shared model or BI/semantic layer. | Fan-out and wide reporting models can be appropriate for the chosen BI tool and consumer needs. Do not use an arbitrary count as an automatic failure. |
| MD-21 | Every model should have visible dependencies through `source()`/`ref()`, unless it is intentionally self-contained, such as a calendar generated by a macro. | Visible lineage allows dbt to order work correctly and lets people assess impact. | A self-contained generated model documents that it has no upstream data dependency. |
| MD-22 | For sources that feed time-sensitive or business-critical outputs, document freshness expectations and monitor them using the repository’s agreed mechanism. | Freshness is an operational expectation of the data product, not merely a model property. | Not every source has a reliable loaded-at field or meaningful SLA; record an alternative monitoring approach or the limitation. |
| MD-23 | Track test and documentation coverage as improvement measures, split by layer or model type where useful. Use the measures to identify risk and prioritise debt, not to reward superficial tests or descriptions. | Portfolio-level visibility prevents invisible quality debt while preserving judgement about meaningful coverage. | Legacy models can be remediated progressively; new or materially changed consumer-facing models follow MD-15. |
| MD-24 | Each repository defines and documents a predictable naming and directory convention that communicates a model’s role, source/domain, and consumer status. | Consistent names and locations improve discoverability for engineers, reviewers, and agents. | The shared kit does not prescribe prefixes such as `stg_`, `int_`, `fct_`, or `dim_`; each repository records its chosen equivalent. |
| MD-25 | Treat long chains of views/ephemeral models, and exposure-facing models that are not physically materialised, as performance-review signals. Select materialisations using observed workload, reuse, query complexity, freshness, and cost. | A deep non-materialised chain can defer significant computation to a final query; heavily consumed outputs need deliberate performance design. | A short chain or low-volume workload may be best as views. Materialising merely to satisfy a count is not the goal. |
| MD-26 | A model deliberately made broadly consumable/public has a declared owner, complete model and column documentation, and a reviewed schema/compatibility approach. Exposures should depend on governed consumer models or metrics, not directly on raw sources or private implementation models. | Public data products require stronger usability and compatibility guarantees than private implementation models. | Whether to use dbt model contracts, another contract mechanism, or documented compatibility controls is a platform and governance decision. |

## Candidate CTE convention to test

This is a candidate *shape*, not a prescribed set of CTE names:

```sql
with

source as (
    select * from {{ source('system', 'entity') }}
),

renamed as (
    select
        cast(id as string) as entity_id,
        updated_at
    from source
),

deduplicated as (
    select ...
    from renamed
),

final as (
    select
        entity_id,
        updated_at
    from deduplicated
)

select * from final
```

Teams should decide whether to adopt this exact pattern, a close variant, or only the underlying principle: CTEs must have a clear role and should make the transformation, grain, and final interface easy to review.

## Illustrative examples for every candidate

These examples show the intended outcome. They are not required names, SQL patterns, or directory layouts.

| ID | Example |
| --- | --- |
| MD-01 | `fct_order_line` says: “one row per order line; unique by `order_id` and `line_number`.” A reviewer can then see whether a later join could multiply order lines. |
| MD-02 | The YAML for `dim_customer` tests `customer_key` as `not_null` and `unique`, because it is the declared one-row-per-customer identifier. |
| MD-03 | A proposal changes `net_revenue` from decimal to integer in `fct_invoice`. The engineer identifies the dashboards and downstream models using it, agrees a migration, and exposes a compatible replacement before removing the old column. |
| MD-04 | `stg_salesforce__account` references only the Salesforce account source, renames `Id` to `account_id`, casts dates, and removes known deleted records. It does not join orders or calculate customer value. |
| MD-05 | `int_customer__latest_subscription` deduplicates subscription revisions and chooses the current record once. Multiple marts then use that agreed definition rather than each implementing their own “latest subscription” rule. |
| MD-06 | `fct_daily_sales` joins already-prepared orders and exchange rates to produce the reporting fact used by Finance. It exposes stable business columns rather than embedding raw-source cleanup. |
| MD-07 | `fct_daily_sales` normally references `int_orders__enriched`, rather than directly joining several raw staging models. If it directly references `stg_exchange_rate` because no transformation is needed, that local pattern is documented. |
| MD-08 | SQL uses `{{ ref('int_customer__latest_subscription') }}` and `{{ source('stripe', 'invoice') }}` rather than `prod_finance.gold.customer_subscription` or a hard-coded Databricks catalog/schema/table name. |
| MD-09 | A model uses `source`, `renamed`, `filtered_valid_orders`, `aggregated`, and `final` CTEs. A reviewer can see where the source is normalised, invalid orders are excluded, and daily totals are calculated. |
| MD-10 | `dim_product` explicitly selects `product_id`, `product_name`, `category`, and `is_active`; a new raw supplier field cannot silently appear in the published dimension. |
| MD-11 | Before joining orders to promotional codes, the model deduplicates promotional-code history to one active record per code. The join therefore remains many-orders-to-one-promotion and preserves one row per order. |
| MD-12 | An order-line model has no source primary key, so it generates `order_line_key` deterministically from `order_id` and `line_number` using the team-approved surrogate-key macro and null-handling convention. |
| MD-13 | A small, source-aligned staging model remains a view under the repository default. A frequently queried, expensive customer-history transformation is materialised as a table after recording its refresh/cost rationale. |
| MD-14 | An incremental events model uses `event_id` as its key, reloads the last three days using source `updated_at`, and documents how late corrections are captured and how a controlled full rebuild would be requested. |
| MD-15 | A new customer mart includes a model description, business definitions for `customer_status` and `lifetime_value`, and tests for its customer key and accepted status values. |
| MD-16 | When a change introduces an `is_eligible` flag, the model includes tests or controlled checks for eligible, ineligible, missing-date, and boundary-date scenarios—not only a generic `not_null` test. |
| MD-17 | The PR says: “Changed `int_customer__latest_subscription`; grain remains one row per customer. Ran targeted build successfully. Did not run full downstream CI. Residual risk: dashboard X has not yet been reconciled against the new status mapping.” |
| MD-18 | Two source YAML entries both point to `raw_crm.account`. The team keeps one canonical `crm.account` source definition, transfers its descriptions/freshness settings, and removes the duplicate alias. |
| MD-19 | A file called `stg_billing__invoice.sql` starts joining `stg_billing__invoice_line`. The team either keeps it source-aligned or renames/moves the combined transformation to its agreed intermediate layer. |
| MD-20 | A reporting model joins nine inputs and is hard to reconcile. The engineer assesses whether two reusable intermediate models would clarify the business concepts, or whether the output is correctly a BI-specific report. The decision is recorded; nothing fails merely because there are nine joins. |
| MD-21 | `dim_calendar` is built only from a date-spine macro. Its documentation says it is intentionally self-contained. In contrast, a hard-coded `prod.raw.orders` reference is replaced with a declared source because it was hiding a real dependency. |
| MD-22 | The orders source drives daily revenue reporting. Its expected arrival is documented as complete by 07:00 each day, with a warning and escalation path when its loaded-at timestamp is late. |
| MD-23 | A monthly quality view shows that consumer marts have meaningful tests on 85% of declared keys, while staging documentation is sparse. The team prioritises the riskiest gaps rather than adding empty descriptions just to reach 100%. |
| MD-24 | One repository chooses `stg_<system>__<entity>`, `int_<domain>__<purpose>`, and `dim_`/`fct_` for marts; another uses different terms. Both document their convention and agents follow the local version. |
| MD-25 | A dashboard queries a mart through five view/ephemeral layers and has become slow. The team measures the query and materialises the expensive, shared intermediate transformation rather than changing every view by default. |
| MD-26 | `fct_monthly_revenue` is made public for other teams. It gains an owner, model/column descriptions, explicit data types or a contract mechanism, and an exposure points to it—not to an internal staging model. |

## Worked layer example

For the same customer domain, the candidate layering rules might result in this flow:

```text
raw CRM account source
        │
        ▼
stg_crm__account
  - rename, cast, source cleanup
        │
        ▼
int_customer__latest_subscription
  - deduplicate subscription history; determine current subscription
        │
        ▼
dim_customer
  - stable, documented customer entity for downstream consumers
```

The important decision is not the specific names. It is that raw-source cleanup, reusable business preparation, and consumer-facing output have clear and non-duplicated owners.

## Research basis

- dbt describes modular SQL models and recommends using `ref()` to manage model dependencies; it does not prescribe one project structure: [SQL models](https://docs.getdbt.com/docs/build/sql-models).
- dbt Labs recommends treating breaking changes to consumed models as migrations, and aligning work to established project conventions: [using dbt for analytics engineering](https://github.com/dbt-labs/dbt-agent-skills/blob/main/skills/dbt/skills/using-dbt-for-analytics-engineering/SKILL.md).
- dbt's public guidance frames staging as a reusable source-facing layer, intermediate as focused preparation, and marts as richer consumer-oriented outputs: [staging models best practices](https://www.getdbt.com/blog/staging-models-best-practices-and-limiting-view-runs) and [modular data modelling](https://www.getdbt.com/blog/modular-data-modeling-techniques).
- GitLab provides a mature public example of documenting model grain, filters, business logic, and caveats, and considers tests and documentation part of a complete model: [GitLab dbt guide](https://handbook.gitlab.com/handbook/enterprise-data/platform/dbt-guide/).
- Fivetran’s public style guide is practical input for readable SQL and CTE conventions: [dbt SQL style guide](https://github.com/fivetran/dbt_style_guide/blob/main/sql_guidelines.md).
- Databricks’ medallion guidance supports progressively improving quality through layers, but does not require any particular dbt folder names: [medallion architecture](https://docs.databricks.com/gcp/en/lakehouse/medallion).
- dbt Project Evaluator provides diagnostic rules across modelling, testing, documentation, structure, performance, and governance. Its ideas are adapted here as review prompts rather than universal gates: [Project Evaluator rules](https://dbt-labs.github.io/dbt-project-evaluator/latest/rules/).
