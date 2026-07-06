# MultiAggJoiner

### *class* skrub.MultiAggJoiner(aux_tables, operations, , keys=None, main_keys=None, aux_keys=None, cols=None, suffixes=None)

Extension of the [`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner) to multiple auxiliary tables.

Apply numerical and categorical aggregation operations on the `cols`
to aggregate, selected by dtypes. See the list of supported operations
at the parameter `operations`.
As opposed to the [`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner), here `aux_tables` is an iterable of tables,
each of which will be joined on the main table.

#### WARNING
The auxiliary table is stored in memory as part of the state of the transformer,
which can lead to high memory usage if the auxiliary table is large.
Consider using the [skrub Data Ops](../../data_opshtml.md#user-guide-data-ops-index) and
a standard dataframe library (Pandas or Polars) to perform the aggregation
instead.

* **Parameters:**
  **aux_tables**
  : Auxiliary dataframes to aggregate then join on the base table.
    The placeholder string “X” can be provided to perform
    self-aggregation on the input data. To provide a single auxiliary table,
    `aux_tables = [table]` is supported, but not `aux_tables = table`.
    It’s possible to provide both the placeholder “X” and auxiliary tables,
    as in `aux_tables = [table, "X"]`. If that’s the case, the second table will
    be replaced by the input data.

  **operations**
  : Aggregation operations to perform on the auxiliary tables.
    <br/>
    Must be an iterable of `operations` for each table in `aux_tables`.
    Supported operations are “count”, “mode”, “min”, “max”, “sum”, “median”,
    “mean”, “std”. The operations “sum”, “median”, “mean”, “std” are reserved
    to numeric type columns.

  **keys**
  : The column names to use for both `main_keys` and `aux_key` when they
    are the same. Provide either `key` or both `main_keys` and `aux_keys`.
    If entries in `keys` contains multiple columns, we will perform
    a multi-column join.
    <br/>
    All `keys` must be present in the main and auxiliary tables before fit.
    It’s not (yet) possible to use columns from the first joined table
    to join the second.
    <br/>
    If not `None`, there must be an iterable of `keys` for each table
    in `aux_tables`.

  **main_keys**
  : Select the columns from the main table to use as keys during
    the join operation.
    If entries in `main_keys` contains multiple columns, we will perform
    a multi-column join.
    <br/>
    If not `None`, there must be an iterable of `main_keys` for each table
    in `aux_tables`.

  **aux_keys**
  : Select the columns from the auxiliary dataframes to use as keys during
    the join operation.
    If entries in `aux_keys` contains multiple columns, we will perform
    a multi-column join.
    <br/>
    All `aux_keys` must be present in respective `aux_tables` before fit.
    It’s not (yet) possible to use columns from the first joined table
    to join the second.
    <br/>
    If not `None`, there must be an iterable of `aux_keys` for each table
    in `aux_tables`.

  **cols**
  : Select the columns from the auxiliary dataframes to use as values during
    the aggregation operations.
    <br/>
    If not `None`, there must be an iterable of `cols` for each table
    in `aux_tables`.
    <br/>
    If set to `None`, `cols` is set to a list of lists. For each table
    in `aux_tables`, the corresponding list will be all columns of that table,
    except the `aux_keys` associated with that table.

  **suffixes**
  : Suffixes to append to the `aux_tables`’ column names.
    If set to `None`, the table indexes in `aux_tables` are used,
    e.g. for an aggregation of 2 `aux_tables`, “_0” and “_1” would be appended
    to column names.

#### SEE ALSO
[`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner)
: Aggregate an auxiliary dataframe before joining it on a base dataframe.

### Notes

If `cols` is not provided, `cols` is set to a list of lists.
For each table in `aux_tables`, the corresponding list will be all columns
of that table, except the `aux_keys` associated with that table.

As opposed to the [`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner), here `aux_tables` is an iterable of tables,
each of which will be joined on the main table. Therefore `aux_keys` is now
an iterable of keys, of the same length as `aux_tables`, and each entry
in `aux_keys` is used to join the corresponding auxiliary table. In the same way,
each entry in `cols` is an iterable of columns to aggregate in the corresponding
auxiliary table. If the keys are the same in the main table and the auxiliary
tables, the `keys` parameter can be used instead of `main_keys` and `aux_keys`.

Therefore if we have a single table, we could either use

- the [`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner): `AggJoiner(aux_table, key="ID")`
- or the [`MultiAggJoiner`](#skrub.MultiAggJoiner): `MultiAggJoiner([aux_table], keys=[["ID"]])`

Note that for `keys`, `main_keys`, `aux_keys`, `cols` and `operations`,
an input of the form `[["a"], ["b"], ["c", "d"]]` is valid
while `["a", "b", ["c", "d"]]` is not.

Using a column from the first auxiliary table to join the second auxiliary table
is not (yet) supported.

Accepts [`pandas.DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame) and `polars.DataFrame` inputs.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import MultiAggJoiner
>>> patients = pd.DataFrame({
...    "patient_id": [1, 2],
...    "age": ["72", "45"],
... })
>>> hospitalizations = pd.DataFrame({
...    "visit_id": range(1, 7),
...    "patient_id": [1, 1, 1, 1, 2, 2],
...    "days_of_stay": [2, 4, 1, 1, 3, 12],
...    "hospital": ["Cochin", "Bichat", "Cochin", "Necker", "Bichat", "Bichat"],
... })
>>> medications = pd.DataFrame({
...    "medication_id": range(1, 6),
...    "patient_id": [1, 1, 1, 1, 2],
...    "medication": ["ozempic", "ozempic", "electrolytes", "ozempic", "morphine"],
... })
>>> glucose = pd.DataFrame({
...    "biology_id": range(1, 7),
...    "patientID": [1, 1, 1, 1, 2, 2],
...    "value": [1.4, 3.4, 1.0, 0.8, 3.1, 6.5],
... })
>>> multi_agg_joiner = MultiAggJoiner(
...    aux_tables=[hospitalizations, medications, glucose],
...    main_keys=[["patient_id"], ["patient_id"], ["patient_id"]],
...    aux_keys=[["patient_id"], ["patient_id"], ["patientID"]],
...    cols=[["days_of_stay"], ["medication"], ["value"]],
...    operations=[["max"], ["mode"], ["mean", "std"]],
...    suffixes=["", "", "_glucose"],
... )
>>> multi_agg_joiner.fit_transform(patients)
   patient_id  age  ...  value_mean_glucose  value_std_glucose
0           1   72  ...                1.65           1.193035
1           2   45  ...                4.80           2.404163
```

The [`MultiAggJoiner`](#skrub.MultiAggJoiner) makes it convenient to aggregate multiple tables, but
the same results could be obtained by chaining 3 separate [`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner):

```pycon
>>> from skrub import AggJoiner
>>> from sklearn.pipeline import make_pipeline
>>> agg_joiner_1 = AggJoiner(
...    aux_table=hospitalizations,
...    key="patient_id",
...    cols="days_of_stay",
...    operations="max",
... )
>>> agg_joiner_2 = AggJoiner(
...    aux_table=medications,
...    key="patient_id",
...    cols="medication",
...    operations="mode",
... )
>>> agg_joiner_3 = AggJoiner(
...    aux_table=glucose,
...    main_key="patient_id",
...    aux_key="patientID",
...    cols="value",
...    operations=["mean", "std"],
...    suffix="_glucose",
... )
>>> pipeline = make_pipeline(agg_joiner_1, agg_joiner_2, agg_joiner_3)
>>> pipeline.fit_transform(patients)
   patient_id  age  ...  value_mean_glucose  value_std_glucose
0           1   72  ...                1.65           1.193035
1           2   45  ...                4.80           2.404163
```

### Methods

| [`fit`](#skrub.MultiAggJoiner.fit)(X[, y])                        | Aggregate auxiliary tables based on the main keys.   |
|-------------------------------------------------------------------|------------------------------------------------------|
| [`fit_transform`](#skrub.MultiAggJoiner.fit_transform)(X[, y])    | Aggregate auxiliary tables based on the main keys.   |
| [`get_params`](#skrub.MultiAggJoiner.get_params)([deep])          | Get parameters for this estimator.                   |
| [`set_output`](#skrub.MultiAggJoiner.set_output)(\*[, transform]) | Set output container.                                |
| [`set_params`](#skrub.MultiAggJoiner.set_params)(\*\*params)      | Set the parameters of this estimator.                |
| [`transform`](#skrub.MultiAggJoiner.transform)(X)                 | Left-join pre-aggregated tables on `X`.              |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Aggregate auxiliary tables based on the main keys.

* **Parameters:**
  **X**
  : Input data, based table on which to left join the
    auxiliary tables.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`MultiAggJoiner`](#skrub.MultiAggJoiner)
  : Fitted [`MultiAggJoiner`](#skrub.MultiAggJoiner) instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

Aggregate auxiliary tables based on the main keys.

* **Parameters:**
  **X**
  : Input data, based table on which to left join the
    auxiliary tables.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  DataFrame
  : The augmented input.

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

Left-join pre-aggregated tables on `X`.

* **Parameters:**
  **X**
  : The input data to transform.
* **Returns:**
  DataFrame
  : The augmented input.

<!-- !! processed by numpydoc !! -->
