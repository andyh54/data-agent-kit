# Data Agent Kit

Shared, versioned references, skills, and future agent profiles to support data engineers following good practices. It is designed to be installed locally by an engineer and used across multiple repositories; each dbt repository continues to own its project-specific instructions, conventions, commands, and data contracts.

## Current scope

Version 0.3 contains four skills and one read-only agent profile:

- `dbt-model-development` sets the minimum expectations for planning, changing, and validating dbt models without assuming a particular project structure or naming convention.
- `dbt-standards-assessment` performs a read-only, evidence-based review of a selected collection of dbt models and writes a Markdown assessment.
- `dbt-sql-server-conversion` translates SQL Server/T-SQL transformation assets into safe, testable dbt designs.
- `dbt-databricks-sql-conversion` translates existing Databricks SQL tables, views, and procedures into appropriate dbt designs.
- `dbt-standards-assessor` invokes the assessment skill and prepares PR-ready assessment evidence for an engineer to check and own.

Installation scripts and the remaining skills will be added after the shared standards have been reviewed. Nothing in this folder is installed into an engineer's editor yet.

The version 0.1 candidate standards sit with the `dbt-model-development` skill, so they travel with the skill when it is installed. They are guidance to review and adapt across the four projects, not automatic pass/fail checks.

## Structure

```text
data-agent-kit/
├── VERSION
├── agents/
│   └── dbt-standards-assessor.agent.md
├── templates/
│   ├── dbt-pull-request-template.md
│   └── repository-assessment-worksheet.md
└── skills/
    ├── dbt-model-development/
        ├── SKILL.md
        └── references/
            ├── dbt-development-standard.md
            ├── modelling-principles.md
            ├── incremental-model-standard.md
            ├── validation-and-change-scope.md
            ├── candidate-modelling-and-development-standards.md
            ├── legacy-sql-conversion-method.md
            └── source-notes.md
    └── dbt-standards-assessment/
        ├── SKILL.md
        └── references/
            ├── assessment-criteria.md
            └── assessment-report-template.md
    ├── dbt-sql-server-conversion/
    │   ├── SKILL.md
    │   └── references/
    │       └── sql-server-to-databricks-considerations.md
    └── dbt-databricks-sql-conversion/
        ├── SKILL.md
        └── references/
            └── databricks-sql-asset-classification.md
```

## How the guidance should be applied

1. Follow security, platform, and repository-specific instructions first.
2. Apply the shared standards in this kit where they do not conflict.
3. Use the dbt Labs skills installed locally when available; their procedural guidance complements this baseline.
4. Escalate ambiguous requirements, possible breaking changes, or unsafe operations rather than guessing.

The future `dbt-databricks-safety` skill will supply the specific rules for production access, cost, destructive operations, schema evolution, and sensitive data.
