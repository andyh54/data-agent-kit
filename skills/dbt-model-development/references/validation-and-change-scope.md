# Validation and change scope

## Proportionate checks

Use the repository-approved commands and development target. Start with the narrowest useful checks: parse/compile, build or test the changed model, then affected downstream models where the repository workflow supports it. Use broader checks when the change's risk warrants them.

Validation is evidence, not a ritual. Match it to the risks introduced:

| Change or risk | Evidence to seek |
| --- | --- |
| New or changed key/grain | uniqueness, required-key, row-count or reconciliation checks where useful |
| New join or deduplication | controlled sample/reconciliation and a test that exposes unexpected multiplicity |
| New business rule | representative positive, negative, and boundary cases |
| Schema/interface change | affected consumer assessment and compatibility/migration plan |
| Incremental change | rerun behaviour, late-data/correction behaviour, and key integrity |

## Documentation is part of the change

The model description should let a data user understand what the model represents. Column descriptions should explain business meaning for fields a user or downstream developer could misunderstand. Record important caveats, calculation rules, and freshness/latency expectations where the project convention provides a place for them.

## What to put in the pull request

After the implementation is complete, include:

1. the outcome and the model(s) changed;
2. the data grain and any interface impact;
3. documentation and tests added/updated, or the reason they were not;
4. validation run and outcome; and
5. assumptions, follow-up work, and residual risks.

This is prepared after the change, so it describes actual evidence rather than a speculative plan.
