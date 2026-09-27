# Assessment criteria

Use these as review prompts. They are derived from the kit’s [version 0.1 candidate standards](../../dbt-model-development/references/candidate-modelling-and-development-standards.md) and dbt Project Evaluator ideas; they are not automatic gates.

| Area | Review prompts | Relevant candidate standards |
| --- | --- | --- |
| Purpose and grain | Is the model's purpose and one-row grain clear? Is its key expectation documented and, where appropriate, tested? | MD-01, MD-02 |
| Consumer interface | Could the model’s name, schema, type, business meaning, or grain affect a consumer? Is public/consumer-facing status clear? | MD-03, MD-26 |
| Layering | Does the model's actual dependency pattern fit the repository's layer responsibilities? Is cleanup centralised and business logic owned once? | MD-04 to MD-07, MD-19 |
| Dependencies and lineage | Are `source()`/`ref()` used? Are sources declared accurately? Is a model’s root/no-parent status intentional? | MD-08, MD-18, MD-21 |
| SQL and joins | Are output columns intentional? Do stages/CTEs make transformation logic clear? Are major joins compatible with the stated grain? | MD-09 to MD-11 |
| Keys and materialisation | Is a stable identifier required and handled appropriately? Is materialisation/ incremental design intentional and explainable? | MD-12 to MD-14, MD-25 |
| Tests and documentation | Are documentation and tests meaningful for the model’s risk? Are source freshness expectations defined where they matter? | MD-15, MD-16, MD-22, MD-23 |
| Project structure | Does naming/directory placement communicate the model’s role under the local convention? | MD-24 |
| DAG complexity | Do fan-out, repeated/rejoined concepts, many joins, or long non-materialised chains indicate a design question worth resolving? | MD-20, MD-25 |
| Governance | Does a broadly consumable model have ownership, documentation, and a schema/compatibility approach? Are exposures connected to governed outputs? | MD-03, MD-26 |

## Priority guide

**Recommendation — now** is appropriate when a reviewed change or existing model plausibly creates incorrect data, unannounced consumer breakage, missing lineage, an ungoverned public interface, or an important unmonitored operational dependency.

**Recommendation — next** is appropriate for a clear maintainability, test, documentation, or consistency improvement that does not need immediate intervention.

**Observe / discuss** is appropriate when a graph pattern or design choice could be intentional, and the evidence available does not establish a defect.
