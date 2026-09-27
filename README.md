# Data Agent Kit

Shared, versioned guidance for teams developing dbt projects. It is designed to be installed locally by an engineer and used across multiple repositories; each dbt repository continues to own its project-specific instructions, conventions, commands, and data contracts.

## Current scope

Version 0.1 contains two skills:

- `dbt-model-development` sets the minimum expectations for planning, changing, and validating dbt models without assuming a particular project structure or naming convention.
- `dbt-standards-assessment` performs a read-only, evidence-based review of a selected collection of dbt models and writes a Markdown assessment.

Agent profiles, installation scripts, and the remaining skills will be added after the shared standards have been reviewed. Nothing in this folder is installed into an engineer's editor yet.

The version 0.1 candidate standards sit with the `dbt-model-development` skill, so they travel with the skill when it is installed. They are guidance to review and adapt across the four projects, not automatic pass/fail checks.

## Structure

```text
data-agent-kit/
├── VERSION
├── templates/
│   └── repository-assessment-worksheet.md
└── skills/
    └── dbt-model-development/
        ├── SKILL.md
        └── references/
            ├── dbt-development-standard.md
            ├── modelling-principles.md
            ├── incremental-model-standard.md
            ├── validation-and-change-scope.md
            ├── candidate-modelling-and-development-standards.md
            └── source-notes.md
    └── dbt-standards-assessment/
        ├── SKILL.md
        └── references/
            ├── assessment-criteria.md
            └── assessment-report-template.md
```

## How the guidance should be applied

1. Follow security, platform, and repository-specific instructions first.
2. Apply the shared standards in this kit where they do not conflict.
3. Use the dbt Labs skills installed locally when available; their procedural guidance complements this baseline.
4. Escalate ambiguous requirements, possible breaking changes, or unsafe operations rather than guessing.

The future `dbt-databricks-safety` skill will supply the specific rules for production access, cost, destructive operations, schema evolution, and sensitive data.
