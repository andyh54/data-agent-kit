# Modelling principles

## Begin with grain

Write the grain in plain language before writing the final query: for example, “one row per customer per calendar day” or “one row per paid invoice.” A model's keys, joins, tests, and consumer expectations must be consistent with that statement.

If the requested outcome needs two incompatible grains, use separate models or explicitly aggregate one input before joining. Do not rely on a `distinct` at the end of a query to conceal a fan-out problem.

## Make transformations intelligible

- Give intermediate stages names that describe their role, rather than their order alone.
- Separate source cleanup, business-rule application, aggregation, and final presentation when that makes the logic easier to inspect.
- Make time-zone assumptions, effective-date logic, default values, and record-selection rules explicit.
- Prefer a small number of readable transformations over a single opaque query or needless abstraction.

## Join deliberately

For each material join, be able to say:

- the intended relationship (one-to-one, many-to-one, and so on);
- which side defines the output grain;
- how unmatched records should behave; and
- how duplicate matches are prevented, accepted, or resolved.

Use an aggregation, qualifying rule, or a documented business decision before a join when the input has more rows than the target grain permits.

## Reuse and ownership

Use an existing well-owned dbt model when it supplies the required business definition. Do not replicate a transformation merely because its table is convenient. When the definition is missing, create it at the layer and ownership boundary used by that repository.

Cross-project or published models should be treated as contracts. Follow the repository's governance/versioning approach and involve the owning team for an interface change.
