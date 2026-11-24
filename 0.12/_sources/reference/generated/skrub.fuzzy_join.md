# fuzzy_join

### skrub.fuzzy_join(left, right, left_on=None, right_on=None, on=None, suffix='', max_dist=inf, ref_dist='random_pairs', string_encoder=Pipeline(steps=[('functiontransformer', FunctionTransformer(func=functools.partial(<function fill_nulls>, value=''))), ('tostr', ToStr()), ('hashingvectorizer', HashingVectorizer(analyzer='char_wb', ngram_range=(2, 4))), ('tfidftransformer', TfidfTransformer())]), add_match_info=False, drop_unmatched=False, metric='euclidean')

Perform a fuzzy (approximate) join between the left and right tables.

Rows in the left table are joined to their closest match from the right
table. The resulting table has the same rows (in the same order) as the
left table, unless `drop_unmatched` is `True`, in which case rows that
are too far from their closest match will not appear in the result. Each
row from the left table appears at most once in the result; if there are
several equally good matching rows in the right table one of them will be
used; which one is unspecified.

#### WARNING
This method can be computationally expensive for large datasets, as it
requires vectorizing the matching columns and performing a nearest neighbor
search.

Additionally, the quality of the matches depends on the choice of the
`string_encoder`, the distance metric and the `ref_dist` used for
rescaling the distances.

* **Parameters:**
  **left**
  : Left operand of the join.

  **right**
  : Right operand of the join.

  **left_on**
  : The column names in the left table on which the join will be performed.
    Can be a string if joining on a single column.
    If `None`, `right_on` must also be `None` and `on` must be provided.

  **right_on**
  : The column names in the right table on which the join will
    be performed. Can be a string if joining on a single column.
    If `None`, `left_on` must also be `None` and `on` must be provided.

  **on**
  : The column names to use for both `left_on` and `right_on` when they
    are the same. Provide either `on` or both `left_on` and `right_on`.

  **suffix**
  : Suffix to append to the `right` table’s column names. You can use it
    to avoid duplicate column names in the join.

  **max_dist**
  : Maximum acceptable (rescaled) distance between a row in the
    `left` table and its nearest neighbor in the `right` table. Rows that
    are farther apart are not considered to match. By default, the distance
    is rescaled so that a value between 0 and 1 is typically a good choice,
    although rescaled distances can be greater than 1 for some choices of
    `ref_dist`. `None`, `"inf"`, `float("inf")` or `numpy.inf`
    mean that no matches are rejected.

  **ref_dist**
  : Options are {“random_pairs”, “second_neighbor”, “self_join_neighbor”,
    “no_rescaling”}. See above for a description of each option. To
    facilitate the choice of `max_dist`, distances between rows in
    `left` table and their nearest neighbor in `right` table will be
    rescaled by this reference distance.

  **string_encoder**
  : By default a `HashingVectorizer` combined with a `TfidfTransformer`
    is used. Here we use raw TF-IDF features rather than transforming them
    for example with `GapEncoder` or `MinHashEncoder` because it is
    faster, these features are only used to find nearest neighbors and not
    used by downstream estimators, and distances between TF-IDF vectors
    have a somewhat simpler interpretation.

  **add_match_info**
  : Insert columns whose names start with `skrub_Joiner` containing
    the distance, rescaled distance and whether the rescaled distance is
    above the threshold. Those values can be helpful for an estimator that
    uses the joined features, or to inspect the result of the join and set
    a `max_dist` threshold.

  **drop_unmatched**
  : Remove rows for which a match was not found in the right table (i.e. for
    which the nearest neighbor is further than `max_dist`).

  **metric**
  : The distance metric to use for nearest neighbor search.
    See [`NearestNeighbors`](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors) for all available metrics.
* **Returns:**
  [`DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame)
  : The joined tables.

#### SEE ALSO
[`Joiner`](skrub.Joinerhtml.md#skrub.Joiner)
: Same as fuzzy_join but as a scikit-learn transformer.

(`left_on` and `right_on`) are vectorized, i.e. represented by vectors of
continuous values. Then, distances between these vectors are computed
(using the specified metric) to find, for each left table row, its nearest
neighbor within the right table.

Optionally, a maximum distance threshold, `max_dist`, can be set. Matches
between vectors that are separated by a distance (strictly) greater than
`max_dist` will be rejected. We will consider that left table rows that
are farther than `max_dist` from their nearest neighbor do not have a
matching row in the right table, and the output will contain nulls for
the entries that would normally have come from the right table (as in a
traditional left join).

To make it easier to set a `max_dist` threshold, the distances are
rescaled by dividing them by a reference distance, which can be chosen with
`ref_dist`. The default is `'random_pairs'`. The possible choices are:

‘random_pairs’
: Pairs of rows are sampled randomly from the right table and their
  distance is computed. The reference distance is the first quartile of
  those distances.

‘second_neighbor’
: The reference distance is the distance to the *second* nearest neighbor
  in the right table.

‘self_join_neighbor’
: Once the match candidate (i.e. the nearest neighbor from the right
  table) has been found, we find its nearest neighbor in the right
  table (excluding itself). The reference distance is the distance that
  separates those 2 right rows.

‘no_rescaling’
: The reference distance is 1.0, i.e. no rescaling of the distances is
  applied.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import fuzzy_join
>>> left_table = pd.DataFrame({"Country": ["France", "Italia", "Georgia"]})
>>> right_table = pd.DataFrame( {"Country": ["Germany", "France", "Italy"],
...                            "Capital": ["Berlin", "Paris", "Rome"]} )
>>> left_table
  Country
0  France
1  Italia
2  Georgia
>>> right_table
   Country Capital
0  Germany  Berlin
1   France   Paris
2    Italy    Rome
>>> fuzzy_join(
...     left_table,
...     right_table,
...     on="Country",
...     suffix="_right",
...     max_dist=0.8,
...     add_match_info=False,
... )
  Country    Country_right    Capital_right
0  France           France            Paris
1  Italia            Italy             Rome
2   Georgia              NaN              NaN
>>> fuzzy_join(
...     left_table,
...     right_table,
...     on="Country",
...     suffix="_right",
...     drop_unmatched=True,
...     max_dist=0.8,
...     add_match_info=False,
... )
  Country    Country_right    Capital_right
0  France           France            Paris
1  Italia            Italy             Rome
>>> fuzzy_join(
...     left_table,
...     right_table,
...     on="Country",
...     suffix="_right",
...     max_dist=float("inf"),
...     add_match_info=False,
... )
  Country    Country_right    Capital_right
0  France           France            Paris
1  Italia           Italy             Rome
2  Georgia          Germany           Berlin
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to combine data from different sources, with a vocabulary not well normalized.">  <div class="sphx-glr-thumbnail-title">Fuzzy joining dirty tables with the Joiner</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Joining tables may be difficult if one entry on one side does not have an exact match on the other side.">  <div class="sphx-glr-thumbnail-title">Spatial join for flight data: Joining across multiple columns</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="We illustrate the InterpolationJoiner, which is a type of join where values from the second table are inferred with machine-learning, rather than looked up in the table. It is useful when exact matches are not available but we have rows that are close enough to make an educated guess -- in this sense it is a generalization of a fuzzy_join.">  <div class="sphx-glr-thumbnail-title">Interpolation join: infer missing rows when joining two tables</div>
</div>
<!-- thumbnail-parent-div-close --></div>
