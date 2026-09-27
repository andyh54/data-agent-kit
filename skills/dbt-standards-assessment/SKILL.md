---
name: dbt-standards-assessment
description: Review a selected collection of dbt models against shared modelling and development standards, then write an evidence-based Markdown assessment with aligned practices, recommendations, and questions. Invoke when a team wants a standards health check; do not use to make model changes.
user-invocable: true
disable-model-invocation: true
---

# dbt standards assessment

Perform a read-only assessment of an explicitly selected collection of dbt models. This skill produces a written assessment; it does not edit SQL/YAML, run destructive commands, or give a pass/fail certification.

Use [assessment criteria](references/assessment-criteria.md) as review prompts and [the report template](references/assessment-report-template.md) for the output. When installed alongside the core skill, also consult `../dbt-model-development/references/` for the detailed shared standards.

## Required scope

Before assessing, establish:

- the repository and its local instructions (`AGENTS.md`, Copilot instructions, project documentation);
- the selected model paths, selector, or domain boundary;
- the purpose of the assessment (baseline, pre-refactor, quality-debt triage, or post-pilot review); and
- the output path and filename.

If the model collection is not specified, ask for it. Do not silently assess an entire large project. If the user does not name an output location, propose `docs/assessments/dbt-standards-assessment-YYYY-MM-DD.md` and obtain confirmation before writing it.

## Evidence to inspect

For each selected model, inspect as available:

1. SQL and its configuration;
2. YAML properties: descriptions, tests, contracts/access, and metadata;
3. direct parents and children in the dbt DAG or manifest;
4. relevant source definitions and freshness settings; and
5. existing repository conventions, linting/configuration, and any supplied validation results.

Read enough directly connected upstream/downstream resources to understand grain, layer role, and interface impact. Do not infer test results, lineage, consumer usage, performance, or data correctness from names alone. Mark unavailable evidence as an observation or question.

## Assessment method

1. Describe the assessment scope and evidence reviewed.
2. Identify practices that align well with the standards; include concrete evidence so the report is useful, not merely critical.
3. Record gaps as one of:
   - **Recommendation — now:** likely correctness, consumer-interface, governance, or material operational risk.
   - **Recommendation — next:** worthwhile consistency, quality, documentation, test, or maintainability improvement.
   - **Observe / discuss:** a design signal requiring local context, not a presumed defect.
4. For every gap, state the relevant standard ID(s), evidence, why it matters, a proportionate recommended action, and any recognised exception.
5. Group repeated issues into a single cross-model recommendation. Do not create separate findings for every occurrence of the same underlying debt.
6. Finish with a prioritised improvement plan and questions that require a human owner or platform decision.

## Boundaries and tone

- The standards are decision support, not a scorecard. Do not calculate a compliance percentage or use “pass”/“fail”.
- Treat model/graph counts (joins, fan-out, view chains, coverage) as prompts for investigation rather than thresholds that automatically prove a problem.
- Preserve established repository conventions where they achieve the same objective; clearly distinguish a shared principle from a local implementation choice.
- Cite model paths and, where useful, line references or manifest evidence. Use precise wording: “not observed in reviewed files” is different from “absent from the project.”
- Never claim that data is correct, performant, safe, or production-ready merely because the static review found no issue.
