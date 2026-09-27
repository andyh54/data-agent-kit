# Data Agent Kit

Shared, versioned references, skills, and agent profiles to support data engineers following good practices. It is designed to be installed locally by an engineer and used across multiple dbt repositories. Each dbt repository continues to own its project-specific instructions, conventions, commands, data contracts, and approved validation commands.

## Current scope

Version 0.3 contains four skills and one read-only agent profile:

- `dbt-model-development` sets the minimum expectations for planning, changing, and validating dbt models without assuming a particular project structure or naming convention.
- `dbt-standards-assessment` performs a read-only, evidence-based review of a selected collection of dbt models and writes a Markdown assessment.
- `dbt-sql-server-conversion` translates SQL Server/T-SQL transformation assets into safe, testable dbt designs.
- `dbt-databricks-sql-conversion` translates existing Databricks SQL tables, views, and procedures into appropriate dbt designs.
- `dbt-standards-assessor` invokes the assessment skill and prepares PR-ready assessment evidence for an engineer to check and own.

This repository intentionally uses a manual installation process first. It is designed for organisations that need to review and distribute approved files rather than allowing engineers to install skills directly from external repositories.

The version 0.1 candidate standards sit with the `dbt-model-development` skill, so they travel with the skill when it is installed. They are guidance to review and adapt across the four projects, not automatic pass/fail checks.

## What an engineer installs

There are two distinct sets of local files:

1. **This kit** supplies your organisation's shared standards, conversion workflows and the read-only standards-assessor agent.
2. **dbt Labs skills** supply specialised dbt procedures, such as analytics engineering, unit testing and documentation. Where direct installation from the dbt Labs repository is not permitted, distribute a reviewed copy of the selected skill folders through your normal internal process.

The two sets complement each other. This kit says what good looks like for your teams; dbt Labs skills provide additional dbt-specific procedures.

## One-time setup — Windows / GitHub Copilot in VS Code

These instructions use the GitHub Copilot user profile, so the skills and agent are available whichever of your dbt repositories an engineer opens.

### 1. Obtain the approved files

Clone or download this repository to a local working location, for example:

```text
C:\src\data-agent-kit
```

Obtain the organisation-approved copy of the selected dbt Labs skill folders. Do not install directly from the internet if your organisation does not permit that. The upstream source is [dbt-labs/dbt-agent-skills](https://github.com/dbt-labs/dbt-agent-skills); retain the upstream version/commit in your internal distribution record.

### 2. Create the Copilot folders

Create these folders if they do not already exist:

```text
C:\Users\<your-user-name>\.copilot\skills
C:\Users\<your-user-name>\.copilot\agents
```

In PowerShell, the equivalent is:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.copilot\skills"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.copilot\agents"
```

### 3. Install this kit's skills and assessor agent

Copy each directory below directly into `C:\Users\<your-user-name>\.copilot\skills\`:

```text
data-agent-kit\skills\dbt-model-development
data-agent-kit\skills\dbt-standards-assessment
data-agent-kit\skills\dbt-sql-server-conversion
data-agent-kit\skills\dbt-databricks-sql-conversion
```

Then copy this file into `C:\Users\<your-user-name>\.copilot\agents\`:

```text
data-agent-kit\agents\dbt-standards-assessor.agent.md
```

The resulting structure must look like this:

```text
C:\Users\<your-user-name>\.copilot\
├── agents\
│   └── dbt-standards-assessor.agent.md
└── skills\
    ├── dbt-model-development\
    │   ├── SKILL.md
    │   └── references\
    ├── dbt-standards-assessment\
    │   ├── SKILL.md
    │   └── references\
    ├── dbt-sql-server-conversion\
    │   ├── SKILL.md
    │   └── references\
    └── dbt-databricks-sql-conversion\
        ├── SKILL.md
        └── references\
```

Copy the **whole skill directory**, not just `SKILL.md`; its `references` folder is required. Also do not add an extra `skills` parent level—for example, `...\.copilot\skills\data-agent-kit\skills\dbt-model-development` is incorrect and may not be discovered.

### 4. Manually install approved dbt Labs skills

For each approved dbt Labs skill, copy the individual skill directory from the reviewed source into the same Copilot skills location. For example:

```text
reviewed-dbt-agent-skills\skills\dbt\skills\using-dbt-for-analytics-engineering
    → C:\Users\<your-user-name>\.copilot\skills\using-dbt-for-analytics-engineering

reviewed-dbt-agent-skills\skills\dbt\skills\adding-dbt-unit-test
    → C:\Users\<your-user-name>\.copilot\skills\adding-dbt-unit-test

reviewed-dbt-agent-skills\skills\dbt\skills\maintaining-dbt-documentation
    → C:\Users\<your-user-name>\.copilot\skills\maintaining-dbt-documentation
```

Again, each destination folder must contain its own `SKILL.md` and all supporting files. Recommended starting set:

- `using-dbt-for-analytics-engineering`
- `adding-dbt-unit-test`
- `maintaining-dbt-documentation`

Add specialised dbt Labs skills only when the team has an agreed need for them. Review their contents and version before approving an update.

### 5. Restart and verify VS Code

Close and reopen VS Code, or run **Developer: Reload Window** from the Command Palette. Open Copilot Chat with the Copilot/Agent Host session target selected.

- Type `/` in chat: installed skills should appear in the menu.
- Open the **Agents** dropdown: `dbt-standards-assessor` should be available.
- If something is missing, open **Chat: Open Customizations** or the chat diagnostics view. Check the exact folder name, `SKILL.md` frontmatter and whether the selected session target is Copilot.

## How engineers use the kit

Always open the relevant dbt repository before using a skill or agent. The repository should supply local guidance—such as model layout, naming, approved dbt commands, development target, data ownership and exceptions—through its `AGENTS.md` and/or `.github/copilot-instructions.md`.

### Tell Copilot to use the locally installed dbt skills

Add an instruction like the following to each dbt repository’s `.github/copilot-instructions.md`, adapting it to local policy:

```md
For dbt planning, model development, SQL conversion, test design, documentation and review work, use relevant locally installed dbt skills when available. Follow this repository's instructions first, then apply the shared data-agent-kit standards. Do not claim a skill, command or validation was used unless it was actually available and run.
```

This does not make skill use deterministic, but it gives Copilot clear, repository-level intent and reminds engineers to invoke the appropriate specialist skill or agent for significant work.

### Build or change a dbt model

In Copilot Chat, invoke the skill from the `/` menu or make an explicit request such as:

```text
Use dbt-model-development to add the customer-order model.
Read this repository’s instructions first. The desired output is one row per customer per day.
Propose the grain, source/ref dependencies, tests, documentation and validation before editing files.
```

### Convert SQL Server T-SQL

```text
Use dbt-sql-server-conversion to analyse this stored procedure and propose its dbt target design.
Do not make changes yet. Identify temp-table steps, parameters, DML, reconciliation checks, anything that belongs outside dbt, and open questions.
```

### Convert existing Databricks SQL

```text
Use dbt-databricks-sql-conversion to assess this Databricks view or procedure for conversion into dbt.
Identify the target model(s), Unity Catalog/security implications, materialisation, tests, reconciliation and any behaviour that must remain in orchestration or Databricks SQL.
```

### Prepare PR assessment evidence

After implementation and approved validation, select **dbt-standards-assessor** from the Copilot Chat **Agents** dropdown. Then ask, for example:

```text
Assess the models changed for this pull request: models/staging/stg_orders.sql and models/marts/fct_orders.sql.
Focus on grain, consumer impact, tests, documentation, layering and incremental behaviour.
Prepare the concise PR assessment summary. Do not edit files.
```

The agent reviews the named scope, reports strengths, recommendations and questions, then prepares the PR summary. The engineer must check, correct and own that text; it is not an automated approval.

## Add PR evidence to each dbt repository

Copy [templates/dbt-pull-request-template.md](templates/dbt-pull-request-template.md) into each dbt project as:

```text
.github/pull_request_template.md
```

GitHub will then pre-populate the PR body. Complete the assessment after implementation and validation, using the assessor’s output as input. For larger migrations, link a full assessment document from the PR; for normal changes, the concise PR summary is sufficient.

## Updating an installation

1. Review the changes in this repository and the approved dbt Labs skill distribution.
2. Update the local `data-agent-kit` clone to the approved version.
3. Replace the corresponding folders under `.copilot\skills` and the assessor file under `.copilot\agents`.
4. Reload VS Code and verify the customizations again.

Do not store credentials, connection profiles, production identifiers or sensitive data in this kit or in skill files.

## macOS / Linux locations

Use the same directory structure under your home folder:

```text
~/.copilot/skills/
~/.copilot/agents/
```

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
