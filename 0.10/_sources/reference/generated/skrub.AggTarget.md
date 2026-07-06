# AggTarget

### *class* skrub.AggTarget(main_key, operations, , suffix='_target')

Aggregate a target `y` before joining its aggregation on a base dataframe.

Accepts [`pandas.DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame) or `polars.DataFrame` inputs.

* **Parameters:**
  **main_key**
  : Select the columns from the main table to use as keys during
    the aggregation of the target and during the join operation.
    <br/>
    If `main_key` refer to a single column, a single aggregation
    for this key will be generated and a single join will be performed.
    <br/>
    If `main_key` is a list of keys, a multi-column aggregation will be performed
    on the target.

  **operations**
  : Aggregation operations to perform on the target.
    <br/>
    Supported operations are “count”, “mode”, “min”, “max”, “sum”, “median”,
    “mean”, “std”. The operations “sum”, “median”, “mean”, “std” are reserved
    to numeric type targets.

  **suffix**
  : The suffix to append to the columns of the target table if the join
    results in duplicates columns.

#### SEE ALSO
[`AggJoiner`](skrub.AggJoinerhtml.md#skrub.AggJoiner)
: Aggregates auxiliary dataframes before joining them on the base dataframe.

[`Joiner`](skrub.Joinerhtml.md#skrub.Joiner)
: Augments a main table by automatically joining multiple auxiliary tables on it.

### Examples

```pycon
>>> import pandas as pd
>>> import numpy as np
>>> from skrub import AggTarget
>>> X = pd.DataFrame({
...     "flightId": range(1, 7),
...     "from_airport": [1, 1, 1, 2, 2, 2],
...     "total_passengers": [90, 120, 100, 70, 80, 90],
...     "company": ["DL", "AF", "AF", "DL", "DL", "TR"],
... })
>>> y = np.array([1, 1, 0, 0, 1, 1])
>>> agg_target = AggTarget(
...     main_key="company",
...     operations=["mean", "max"],
... )
>>> agg_target.fit_transform(X, y)
   flightId  from_airport  ...  y_0_mean_target  y_0_max_target
0         1             1  ...         0.666667               1
1         2             1  ...         0.500000               1
2         3             1  ...         0.500000               1
3         4             2  ...         0.666667               1
4         5             2  ...         0.666667               1
5         6             2  ...         1.000000               1
```

### Methods

| [`fit`](#skrub.AggTarget.fit)(X, y)                                 | Aggregate the target `y` based on keys from `X`.   |
|---------------------------------------------------------------------|----------------------------------------------------|
| [`fit_transform`](#skrub.AggTarget.fit_transform)(X, y)             | Aggregate the target `y` based on keys from `X`.   |
| [`get_feature_names_out`](#skrub.AggTarget.get_feature_names_out)() | Get output feature names for transformation.       |
| [`get_params`](#skrub.AggTarget.get_params)([deep])                 | Get parameters for this estimator.                 |
| [`set_output`](#skrub.AggTarget.set_output)(\*[, transform])        | Set output container.                              |
| [`set_params`](#skrub.AggTarget.set_params)(\*\*params)             | Set the parameters of this estimator.              |
| [`transform`](#skrub.AggTarget.transform)(X)                        | Left-join pre-aggregated target on `X`.            |
<!-- !! processed by numpydoc !! -->

#### fit(X, y)

Aggregate the target `y` based on keys from `X`.

* **Parameters:**
  **X**
  : Must contains the columns names defined in `main_key`.

  **y**
  : `y` length must match `X` length.
    The target can be continuous or discrete, with multiple columns.
* **Returns:**
  AggTarget
  : Fitted [`AggTarget`](#skrub.AggTarget) instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y)

Aggregate the target `y` based on keys from `X`.

* **Parameters:**
  **X**
  : Must contains the columns names defined in `main_key`.

  **y**
  : `y` length must match `X` length.
    The target can be continuous or discrete, with multiple columns.
* **Returns:**
  Dataframe
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

Left-join pre-aggregated target on `X`.

* **Parameters:**
  **X**
  : The input data to transform.
* **Returns:**
  **X_transformed**
  : The augmented input.

<!-- !! processed by numpydoc !! -->
