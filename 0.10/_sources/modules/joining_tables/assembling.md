# Assembling: joining multiple tables

Assembling is the process of collecting and joining together tables. Good analytics
requires including as much information as possible, often from different sources.

Skrub allows you to join tables on keys of different types (string, numerical,
datetime) with imprecise correspondence.

#### WARNING
To be considered when using one of the joiners:

**Joiners are designed for small-to-medium datasets.**

- **Memory**: The auxiliary table is stored in the transformer state.
  For tables > 1 million rows, consider using [skrub Data Ops](../../data_opshtml.md#user-guide-data-ops-index) with pandas/polars joins instead.
- **Computational Cost**: Fuzzy joining requires vectorizing columns
  and nearest-neighbor search. Test on samples first for large datasets.
- **Dynamic Data**: If your auxiliary table changes after fitting,
  you must refit the transformer. Joiners are not suitable for continuously
  updated tables.

## Joining external tables for machine learning

Joining is straightforward for two tables because you only need to identify
the common key.

In addition, skrub also enable more advanced analysis:

- [`Joiner`](../../reference/generated/skrub.Joinerhtml.md#skrub.Joiner): fuzzy-joins an external table using a scikit-learn
  transformer, which can be used in a scikit-learn [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline).
  Pipelines are useful for cross-validation and hyper-parameter search, but also
  for model deployment.
- [`AggJoiner`](../../reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner): instead of performing 1:1 joins like [`Joiner`](../../reference/generated/skrub.Joinerhtml.md#skrub.Joiner),
  [`AggJoiner`](../../reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner)
  aggregates the external table first, then joins it on the main table.
  Alternatively, it can aggregate the main table and then join it back onto itself.
- [`AggTarget`](../../reference/generated/skrub.AggTargethtml.md#skrub.AggTarget): in some settings, one can derive powerful features from
  the target `y` itself. AggTarget aggregates the target without risking data
  leakage, then joins the result back on the main table, similar to AggJoiner.
- [`MultiAggJoiner`](../../reference/generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner): extension of the [`AggJoiner`](../../reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) that joins multiple
  auxiliary tables onto the main table.

## Fuzzy joining tables

Joining two dataframes can be hard as the corresponding keys may be different.

[`fuzzy_join()`](../../reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) uses similarities in entries to join tables on one or more
related columns. Furthermore, it chooses the type of fuzzy matching used based
on the column type (string, numeric or datetime). It also outputs a similarity
score, to single out bad matches, so that they can be dropped or replaced.

In sum, equivalent to [`pandas.merge()`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.merge.html#pandas.merge), the [`fuzzy_join()`](../../reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join)
has no need for pre-cleaning.

### Using the [`InterpolationJoiner`](../../reference/generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) to join tables using ML predictions

The [`InterpolationJoiner`](../../reference/generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) is a transformer that performs an operation similar
to that of a regular equi-join, but that can handle the presence of missing rows
in the right table (the table to be added). This is done by estimating the value
that the missing rows would have by training a machine learning model on the data
we have access to.

This transformer is explored in more detail in [this example](../../auto_examples/03_joining/0080_interpolation_joinhtml.md#sphx-glr-auto-examples-03-joining-0080-interpolation-join-py).
