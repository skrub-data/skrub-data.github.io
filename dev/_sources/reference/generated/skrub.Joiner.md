# Joiner

### *class* skrub.Joiner(aux_table, \*, key=None, main_key=None, aux_key=None, suffix='', max_dist=inf, ref_dist='random_pairs', string_encoder=Pipeline(steps=[('functiontransformer', FunctionTransformer(func=functools.partial(<function fill_nulls>, value=''))), ('tostr', ToStr()), ('hashingvectorizer', HashingVectorizer(analyzer='char_wb', ngram_range=(2, 4))), ('tfidftransformer', TfidfTransformer())]), add_match_info=True, metric='euclidean')

Augment features in a main table by fuzzy-joining an auxiliary table to it.

This transformer is initialized with an auxiliary table `aux_table`. It
transforms a main table by joining it, with approximate (“fuzzy”) matching,
to the auxiliary table. The output of `transform` has the same rows as
the main table (i.e. as the argument passed to `transform`), but each row
is augmented with values from the best match in the auxiliary table.

Fuzzy joining is performed by finding the nearest neighbor in the auxiliary
table for each row in the main table according to a specified distance metric.

#### WARNING
This method can be computationally expensive for large datasets, as it
requires vectorizing the matching columns and performing nearest neighbor
search.

Additionally, the auxiliary table is stored in memory as part of the state
of the transformer, which can lead to high memory usage if the auxiliary
table is large.
Moreover, the auxiliary table is frozen in memory after fitting, which means
that if it is modified after fitting, the changes will not be reflected
in the transformed output. If you need to update the auxiliary table, you
will need to refit the transformer.

* **Parameters:**
  **aux_table**
  : The auxiliary table, which will be fuzzy-joined to the main table when
    calling `transform`.

  **key**
  : The column names to use for both `main_key` and `aux_key` when they
    are the same. Provide either `key` or both `main_key` and `aux_key`.

  **main_key**
  : The column names in the main table on which the join will be performed.
    Can be a string if joining on a single column.
    If `None`, `aux_key` must also be `None` and `key` must be provided.

  **aux_key**
  : The column names in the auxiliary table on which the join will
    be performed. Can be a string if joining on a single column.
    If `None`, `main_key` must also be `None` and `key` must be provided.

  **suffix**
  : Suffix to append to the `aux_table`’s column names. You can use it
    to avoid duplicate column names in the join.

  **max_dist**
  : Maximum acceptable (rescaled) distance between a row in the
    `main_table` and its nearest neighbor in the `aux_table`. Rows that
    are farther apart are not considered to match. By default, the distance
    is rescaled so that a value between 0 and 1 is typically a good choice,
    although rescaled distances can be greater than 1 for some choices of
    `ref_dist`. `None`, `"inf"`, `float("inf")` or `numpy.inf`
    mean that no matches are rejected.

  **ref_dist**
  : Options are {“random_pairs”, “second_neighbor”, “self_join_neighbor”,
    “no_rescaling”}. See above for a description of each option. To
    facilitate the choice of `max_dist`, distances between rows in
    `main_table` and their nearest neighbor in `aux_table` will be
    rescaled by this reference distance.

  **string_encoder**
  : By default a `HashingVectorizer` combined with a `TfidfTransformer`
    is used. Here we use raw TF-IDF features rather than transforming them
    for example with `GapEncoder` or `MinHashEncoder` because it is
    faster, these features are only used to find nearest neighbors and not
    used by downstream estimators, and distances between TF-IDF vectors
    have a somewhat simpler interpretation.

  **add_match_info**
  : Insert some columns whose names start with `skrub_Joiner` containing
    the distance, rescaled distance and whether the rescaled distance is
    above the threshold. Those values can be helpful for an estimator that
    uses the joined features, or to inspect the result of the join and set
    a `max_dist` threshold.

  **metric**
  : The distance metric to use for nearest neighbor search.
    See [`NearestNeighbors`](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors) for all available metrics.
* **Attributes:**
  **max_dist_**
  : Equal to the parameter `max_dist` except that `"inf"` and `None`
    are mapped to `np.inf` (i.e. accept all matches).

  **vectorizer_**
  : The fitted transformer used to transform the matching columns into
    numerical vectors.

#### SEE ALSO
[`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner)
: Aggregate an auxiliary dataframe before joining it on a base dataframe.

[`fuzzy_join`](skrub.fuzzy_joinhtml.md#skrub.fuzzy_join)
: Join two tables (dataframes) based on approximate column matching. This is the same functionality as provided by the `Joiner` but exposed as a function rather than a transformer.

### Notes

This transformer is initialized with an auxiliary table `aux_table`. It
transforms a main table by joining it, with approximate (“fuzzy”) matching,
to the auxiliary table. The output of `transform` has the same rows as
the main table (i.e. as the argument passed to `transform`), but each row
is augmented with values from the best match in the auxiliary table.

To identify the best match for each row, values from the matching columns
(`main_key` and `aux_key`) are vectorized, i.e. represented by vectors of
continuous values. Then, the distances between these vectors are computed
(using the specified metric) to find, for each main table row, its
nearest neighbor within the auxiliary table.

Optionally, a maximum distance threshold, `max_dist`, can be set. Matches
between vectors that are separated by a distance (strictly) greater than
`max_dist` will be rejected. We will consider that main table rows that
are farther than `max_dist` from their nearest neighbor do not have a
matching row in the auxiliary table, and the output will contain nulls for
the entries that would normally have come from the auxiliary table (as in a
traditional left join).

To make it easier to set a `max_dist` threshold, the distances are
rescaled by dividing them by a reference distance, which can be chosen with
`ref_dist`. The default is `'random_pairs'`. The possible choices are:

‘random_pairs’
: Pairs of rows are sampled randomly from the auxiliary table and their
  distance is computed. The reference distance is the first quartile of
  those distances.

‘second_neighbor’
: The reference distance is the distance to the *second* nearest neighbor
  in the auxiliary table.

‘self_join_neighbor’
: Once the match candidate (i.e. the nearest neighbor from the auxiliary
  table) has been found, we find its nearest neighbor in the auxiliary
  table (excluding itself). The reference distance is the distance that
  separates those 2 auxiliary rows.

‘no_rescaling’
: The reference distance is 1.0, i.e. no rescaling of the distances is
  applied.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import Joiner
>>> main_table = pd.DataFrame({"Country": ["France", "Italia", "Georgia"]})
>>> aux_table = pd.DataFrame( {"Country": ["Germany", "France", "Italy"],
...                            "Capital": ["Berlin", "Paris", "Rome"]} )
>>> main_table
  Country
0  France
1  Italia
2   Georgia
>>> aux_table
   Country Capital
0  Germany  Berlin
1   France   Paris
2    Italy    Rome
>>> joiner = Joiner(
...     aux_table,
...     key="Country",
...     suffix="_aux",
...     max_dist=0.8,
...     add_match_info=False,
... )
>>> joiner.fit_transform(main_table)
  Country      Country_aux      Capital_aux
0  France           France            Paris
1  Italia            Italy             Rome
2  Georgia              NaN              NaN
```

### Methods

| [`fit`](#skrub.Joiner.fit)(X[, y])                        | Fit the instance to the main table.                |
|-----------------------------------------------------------|----------------------------------------------------|
| [`fit_transform`](#skrub.Joiner.fit_transform)(X[, y])    | Fit the instance to the main table.                |
| [`get_params`](#skrub.Joiner.get_params)([deep])          | Get parameters for this estimator.                 |
| [`set_output`](#skrub.Joiner.set_output)(\*[, transform]) | Set output container.                              |
| [`set_params`](#skrub.Joiner.set_params)(\*\*params)      | Set the parameters of this estimator.              |
| [`transform`](#skrub.Joiner.transform)(X[, y])            | Transform `X` using the specified encoding scheme. |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit the instance to the main table.

* **Parameters:**
  **X**
  : The main table, to be joined to the auxiliary ones.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`Joiner`](#skrub.Joiner)
  : Fitted :class:
    <br/>
    ```
    `
    ```
    <br/>
    Joiner\`instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

Fit the instance to the main table.

* **Parameters:**
  **X**
  : The main table, to be joined to the auxiliary ones.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  DataFrame
  : The final joined table.

<!-- !! processed by numpydoc !! -->

#### get_params(deep=True)

Get parameters for this estimator.

* **Parameters:**
  **deep**
  : If True, will return the parameters for this estimator and
    contained subobjects that are estimators.
* **Returns:**
  **params**
  : Parameter names mapped to their values.

<!-- !! processed by numpydoc !! -->

#### set_output(, transform=None)

Set output container.

Refer to the [user guide](https://scikit-learn.org/stable/modules/df_output_transform.html#df-output-transform) for more details
and [Introducing the set_output API](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_set_output.html#sphx-glr-auto-examples-miscellaneous-plot-set-output-py) for an
example on how to use the API.

* **Parameters:**
  **transform**
  : Configure output of `transform` and `fit_transform`.
    - `"default"`: Default output format of a transformer
    - `"pandas"`: DataFrame output
    - `"polars"`: Polars output
    - `None`: Transform configuration is unchanged
    <br/>
    #### Versionadded
    Added in version 1.4: `"polars"` option was added.
* **Returns:**
  **self**
  : Estimator instance.

<!-- !! processed by numpydoc !! -->

#### set_params(\*\*params)

Set the parameters of this estimator.

The method works on simple estimators as well as on nested objects
(such as [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)). The latter have
parameters of the form `<component>__<parameter>` so that it’s
possible to update each component of a nested object.

* **Parameters:**
  **\*\*params**
  : Estimator parameters.
* **Returns:**
  **self**
  : Estimator instance.

<!-- !! processed by numpydoc !! -->

#### transform(X, y=None)

Transform `X` using the specified encoding scheme.

* **Parameters:**
  **X**
  : The main table, to be joined to the auxiliary ones.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  dataframe
  : The final joined table.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to combine data from different sources, with a vocabulary not well normalized.">  <div class="sphx-glr-thumbnail-title">Fuzzy joining dirty tables with the Joiner</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Joining tables may be difficult if one entry on one side does not have an exact match on the other side.">  <div class="sphx-glr-thumbnail-title">Spatial join for flight data: Joining across multiple columns</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
