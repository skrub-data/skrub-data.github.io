<a id="joining-ref"></a>

# Joining dataframes

| [`Joiner`](generated/skrub.Joinerhtml.md#skrub.Joiner)                                        | Augment features in a main table by fuzzy-joining an auxiliary table to it.                                    |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [`AggJoiner`](generated/skrub.AggJoinerhtml.md#skrub.AggJoiner)                               | Aggregate an auxiliary dataframe before joining it on a base dataframe.                                        |
| [`MultiAggJoiner`](generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner)                | Extension of the [`AggJoiner`](generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) to multiple auxiliary tables. |
| [`AggTarget`](generated/skrub.AggTargethtml.md#skrub.AggTarget)                               | Aggregate a target `y` before joining its aggregation on a base dataframe.                                     |
| [`InterpolationJoiner`](generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) | Join with a table augmented by machine-learning predictions.                                                   |
| [`fuzzy_join`](generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join)                            | Perform a fuzzy (approximate) join between the left and right tables.                                          |
