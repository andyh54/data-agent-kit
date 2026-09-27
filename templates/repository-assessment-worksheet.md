# Repository assessment worksheet

**Purpose:** test the candidate modelling and development standards against real work in all four dbt projects before adopting them as shared agent guidance.

**Use one copy of this worksheet per repository.** Choose two or three representative models:

- a straightforward source/staging model;
- a complex intermediate or transformation model; and
- a consumer-facing mart or incremental model.

Avoid selecting only the team’s best examples. Include one model that is difficult to maintain or has caused a review, quality, performance, or documentation problem.

## Repository and reviewers

| Field | Entry |
| --- | --- |
| Team / repository | |
| Participants | |
| Date assessed | |
| Local layer names and purpose | |
| Existing naming/key/materialisation conventions | |
| Existing dbt tests and documentation convention | |

## Model sample

| Model | Role/layer | Why selected | Has consumers? | Main concern |
| --- | --- | --- | --- | --- |
| | | | | |
| | | | | |
| | | | | |

## Candidate-rule assessment

For each candidate ID in `candidate-modelling-and-development-standards.md`, record one of: **adopt as written**, **adapt wording**, **repository-specific**, **not suitable**, or **need evidence**.

| Candidate ID | Decision | Evidence from selected model(s) | Proposed wording or exception | Owner / action |
| --- | --- | --- | --- | --- |
| MD-01 | | | | |
| MD-02 | | | | |
| MD-03 | | | | |
| MD-04 | | | | |
| MD-05 | | | | |
| MD-06 | | | | |
| MD-07 | | | | |
| MD-08 | | | | |
| MD-09 | | | | |
| MD-10 | | | | |
| MD-11 | | | | |
| MD-12 | | | | |
| MD-13 | | | | |
| MD-14 | | | | |
| MD-15 | | | | |
| MD-16 | | | | |
| MD-17 | | | | |
| MD-18 | | | | |
| MD-19 | | | | |
| MD-20 | | | | |
| MD-21 | | | | |
| MD-22 | | | | |
| MD-23 | | | | |
| MD-24 | | | | |
| MD-25 | | | | |
| MD-26 | | | | |

## CTE-pattern exercise

Take one non-trivial model and assess the proposed CTE shape.

- Does the model’s CTE structure reveal inputs, cleanup, transformation, and final output?
- Would the proposed pattern improve reviewability, or add unnecessary ceremony?
- Which names and exceptions would be natural for this repository?
- Does the preferred linter configuration support the pattern?

**Decision:**

## Cross-team consolidation

After all four worksheets are complete, capture each rule in one of three outcomes:

| Outcome | Meaning | Destination |
| --- | --- | --- |
| Shared mandatory standard | Applies across every team unless an approved exception is documented. | Relevant `skills/dbt-model-development/references/*.md` file |
| Shared principle, local implementation | The outcome is common but names, folders, or commands differ. | Shared reference plus each repository’s `AGENTS.md`/Copilot instructions |
| Repository-specific convention | Valid only in one project because of its architecture, data, or operating model. | That repository’s guidance only |

Do not add an agent instruction simply because it is common. Add it when the teams agree it is clear, valuable, reviewable, and practical in their normal development flow.
