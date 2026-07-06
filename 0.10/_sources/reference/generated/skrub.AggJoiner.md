# AggJoiner

### *class* skrub.AggJoiner(aux_table, operations, , key=None, main_key=None, aux_key=None, cols=None, suffix='')

Aggregate an auxiliary dataframe before joining it on a base dataframe.

Apply numerical and categorical aggregation operations on the columns (i.e. `cols`)
to aggregate. See the list of supported operations at the parameter `operations`.

If `cols` is not provided, `cols` are all columns from `aux_table`,
except `aux_key`.

Accepts [`pandas.DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame) and `polars.DataFrame` inputs.

#### WARNING
The auxiliary table is stored in memory as part of the state of the transformer,
which can lead to high memory usage if the auxiliary table is large.

Additionally, the auxiliary table is frozen in memory after fitting, which
means that if the auxiliary table is modified after fitting, the changes will
not be reflected in the transformed output. If you need to update the
auxiliary table, you will need to refit the transformer.

Consider using the [skrub Data Ops](../../data_opshtml.md#user-guide-data-ops-index)
and a standard dataframe library (Pandas or Polars) to perform the
aggregation instead.

* **Parameters:**
  **aux_table**
  : Auxiliary dataframe to aggregate then join on the base table.
    The placeholder string “X” can be provided to perform
    self-aggregation on the input data.

  **operations**
  : Aggregation operations to perform on the auxiliary table.
    <br/>
    Supported operations are “count”, “mode”, “min”, “max”, “sum”, “median”,
    “mean”, “std”. The operations “sum”, “median”, “mean”, “std” are reserved
    to numeric type columns.

  **key**
  : The column name to use for both `main_key` and `aux_key` when they
    are the same. Provide either `key` or both `main_key` and `aux_key`.
    If `key` is an iterable, we will perform a multi-column join.

  **main_key**
  : Select the columns from the main table to use as keys during
    the join operation.
    If `main_key` is an iterable, we will perform a multi-column join.

  **aux_key**
  : Select the columns from the auxiliary dataframe to use as keys during
    the join operation.
    If `aux_key` is an iterable, we will perform a multi-column join.

  **cols**
  : Select the columns from the auxiliary dataframe to use as values during
    the aggregation operations.
    By default, `cols` are all columns from `aux_table`, except `aux_key`.

  **suffix**
  : Suffix to append to the `aux_table`’s column names. You can use it
    to avoid duplicate column names in the join.

#### SEE ALSO
[`AggTarget`](skrub.AggTargethtml.md#skrub.AggTarget)
: Aggregates the target `y` before joining its aggregation on the base dataframe.

[`Joiner`](skrub.Joinerhtml.md#skrub.Joiner)
: Augments a main table by automatically joining an auxiliary table on it.

[`MultiAggJoiner`](skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner)
: Extension of the AggJoiner to multiple auxiliary tables.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import AggJoiner
>>> main = pd.DataFrame({
...     "airportId": [1, 2],
...     "airportName": ["Paris CDG", "NY JFK"],
... })
>>> aux = pd.DataFrame({
...     "flightId": range(1, 7),
...     "from_airport": [1, 1, 1, 2, 2, 2],
...     "total_passengers": [90, 120, 100, 70, 80, 90],
...     "company": ["DL", "AF", "AF", "DL", "DL", "TR"],
... })
>>> agg_joiner = AggJoiner(
...     aux_table=aux,
...     operations="mean",
...     main_key="airportId",
...     aux_key="from_airport",
...     cols="total_passengers",
... )
>>> agg_joiner.fit_transform(main)
   airportId  airportName  total_passengers_mean
0          1    Paris CDG              103.33...
1          2       NY JFK               80.00...
```

### Methods

| [`fit`](#skrub.AggJoiner.fit)(X[, y])                               | Aggregate auxiliary table based on the main keys.   |
|---------------------------------------------------------------------|-----------------------------------------------------|
| [`fit_transform`](#skrub.AggJoiner.fit_transform)(X[, y])           | Aggregate auxiliary table based on the main keys.   |
| [`get_feature_names_out`](#skrub.AggJoiner.get_feature_names_out)() | Get output feature names for transformation.        |
| [`get_params`](#skrub.AggJoiner.get_params)([deep])                 | Get parameters for this estimator.                  |
| [`set_output`](#skrub.AggJoiner.set_output)(\*[, transform])        | Set output container.                               |
| [`set_params`](#skrub.AggJoiner.set_params)(\*\*params)             | Set the parameters of this estimator.               |
| [`transform`](#skrub.AggJoiner.transform)(X)                        | Left-join pre-aggregated table on `X`.              |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Aggregate auxiliary table based on the main keys.

* **Parameters:**
  **X**
  : Input data, based table on which to left join the
    auxiliary table.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`AggJoiner`](#skrub.AggJoiner)
  : Fitted [`AggJoiner`](#skrub.AggJoiner) instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

Aggregate auxiliary table based on the main keys.

* **Parameters:**
  **X**
  : Input data, based table on which to left join the
    auxiliary table.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  DataFrame
  : The augmented input.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out()

Get output feature names for transformation.

* **Returns:**
  List of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : Transformed feature names.

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

#### transform(X)

Left-join pre-aggregated table on `X`.

* **Parameters:**
  **X**
  : The input data to transform.
* **Returns:**
  DataFrame
  : The augmented input.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
