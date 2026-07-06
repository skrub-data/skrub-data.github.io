# InterpolationJoiner

### *class* skrub.InterpolationJoiner(aux_table, , key=None, main_key=None, aux_key=None, suffix='', regressor=HistGradientBoostingRegressor(), classifier=HistGradientBoostingClassifier(), vectorizer=TableVectorizer(high_cardinality=MinHashEncoder()), n_jobs=None, on_estimator_failure='warn')

Join with a table augmented by machine-learning predictions.

This is similar to a usual equi-join, but instead of looking for actual
rows in the right table that satisfy the join condition, we estimate what
those rows would contain if they existed in the table.

* **Parameters:**
  **aux_table**
  : The (auxiliary) table to be joined to the `main_table` (which is the
    argument of `transform`). `aux_table` is used to train a model that
    takes as inputs the contents of the columns listed in `aux_key`, and
    predicts the contents of the other columns. In the example above, we
    want our transformer to add temperature data to the table it is
    operating on. Therefore, `aux_table` is the `annual_avg_temp`
    table.

  **key**
  : Column names to use for both `main_key` and `aux_key`, when they are
    the same. Provide either `key` (only) or both `main_key` and `aux_key`.

  **main_key**
  : The columns in the main table used for joining. The main table is the
    argument of `transform`, to which we add information inferred using
    `aux_table`. The column names listed in `main_key` will provide the
    inputs (features) of the interpolators at prediction (joining) time. In
    the example above, `main_key` is `["latitude", "longitude"]`, which
    refer to columns in the `buildings` table. When joining on a single
    column, we can pass its name rather than a list: `"latitude"` is
    equivalent to `["latitude"]`.

  **aux_key**
  : The columns in `aux_table` used for joining. Their number and types
    must match those of the `main_key` columns in the main table. These
    columns provide the features for the estimators to be fitted. As for
    `main_key`, it is possible to pass a string when using a single
    column.

  **suffix**
  : Suffix to append to the `aux_table`’s column names. If duplicate column
    names are found, a \_\_skrub_<random string>_\_ is added at the end of columns
    that would otherwise be duplicates.

  **regressor**
  : Model used to predict the numerical columns of `aux_table`.

  **classifier**
  : Model used to predict the string and categorical columns of `aux_table`.

  **vectorizer**
  : Used to transform the feature columns before passing them to the
    scikit-learn estimators. This is useful if we are joining on columns
    that need some transformation, such as dates or strings representing
    high-cardinality categories. By default we use a `MinHashEncoder` to
    vectorize text columns. This is because the `MinHashEncoder` is very
    fast and usually gives good results with downstream learners based on
    trees like the gradient-boosted trees used by default for `regressor`
    and `classifier`. If you replace the default regressor and classifier
    with models such as nearest-neighbors or linear models, consider
    passing `vectorizer=TableVectorizer()` which will encode text with a
    `GapEncoder` rather than a `MinHashEncoder`.

  **n_jobs**
  : Number of jobs to run in parallel. `None` means 1 unless in a
    `joblib.parallel_backend` context. -1 means using all processors.
    Depending on the estimators used and the contents of `aux_table`,
    several estimators may need to be fitted – for example one for
    continuous outputs (regressor) and one for categorical outputs
    (classifier), or one for each column when the provided estimators do
    not support multi-output tasks. Fitting and querying these estimators
    can be done in parallel.

  **on_estimator_failure**
  : How to handle exceptions raised when fitting one of the estimators
    (regressors and classifiers) or querying them for a prediction. If
    “raise”, exceptions are propagated. If “pass” (i) if an exception is
    raised during `fit` the corresponding columns are ignored – they
    will not appear in the join and (ii) if an exception is raised during
    `transform`, the corresponding column will be filled with nulls.
    Columns are filled with nulls during `transform` rather than dropped
    so that the output always has the same shape. If “warn” (the default),
    behave like “pass” but issue a warning.
* **Attributes:**
  **vectorizer_**
  : The transformer used to vectorize the feature columns.

  **estimators_**
  : The estimators used to infer values to be joined. Each entry in this
    list is a dictionary with keys `"estimator"` (the fitted estimator)
    and `"columns"` (the list of columns in `aux_table` that it is
    trained to predict).

#### SEE ALSO
[`Joiner`](skrub.Joinerhtml.md#skrub.Joiner)
: Works in a similar way but instead of inferring values, picks the closest row from the auxiliary table.

### Notes

Suppose we want to join a table `buildings(latitude, longitude, n_stories)`
with a table `annual_avg_temp(latitude, longitude, avg_temp)`. Our annual
average temperature table may not contain data for the exact latitude and
longitude of our buildings. However, we can interpolate what we need from
the data points it does contain. Using `annual_avg_temp`, we train a
model to predict the temperature, given the latitude and longitude. Then,
we use this model to estimate the values we want to add to our
`buildings` table. In a way we are joining `buildings` to a virtual
table, in which rows for any (latitude, longitude) location are inferred,
rather than retrieved, when requested. This is done with:

```default
InterpolationJoiner(
    annual_avg_temp, on=["latitude", "longitude"]
).fit_transform(buildings)
```

### Examples

```pycon
>>> import pandas as pd
>>> buildings = pd.DataFrame(
...     {"latitude": [1.0, 2.0], "longitude": [1.0, 2.0], "n_stories": [3, 7]}
... )
>>> annual_avg_temp = pd.DataFrame(
...     {
...         "latitude": [1.2, 0.9, 1.9, 1.7, 5.0],
...         "longitude": [0.8, 1.1, 1.8, 1.8, 5.0],
...         "avg_temp": [10.0, 11.0, 15.0, 16.0, 20.0],
...     }
... )
>>> buildings
   latitude  longitude  n_stories
0       1.0        1.0          3
1       2.0        2.0          7
>>> annual_avg_temp
   latitude  longitude  avg_temp
0       1.2        0.8      10.0
1       0.9        1.1      11.0
2       1.9        1.8      15.0
3       1.7        1.8      16.0
4       5.0        5.0      20.0
```

Let’s interpolate the average temperature:

```pycon
>>> from sklearn.neighbors import KNeighborsRegressor
>>> from skrub import InterpolationJoiner
>>> InterpolationJoiner(
...     annual_avg_temp,
...     key=["latitude", "longitude"],
...     regressor=KNeighborsRegressor(2),
... ).fit_transform(buildings)
   latitude  longitude  n_stories  avg_temp
0       1.0        1.0          3      10.5
1       2.0        2.0          7      15.5
```

### Methods

| [`fit`](#skrub.InterpolationJoiner.fit)(X[, y])                        | Fit estimators to the `aux_table` provided during initialization.   |
|------------------------------------------------------------------------|---------------------------------------------------------------------|
| [`fit_transform`](#skrub.InterpolationJoiner.fit_transform)(X[, y])    | Fit to data, then transform it.                                     |
| [`get_params`](#skrub.InterpolationJoiner.get_params)([deep])          | Get parameters for this estimator.                                  |
| [`set_output`](#skrub.InterpolationJoiner.set_output)(\*[, transform]) | Set output container.                                               |
| [`set_params`](#skrub.InterpolationJoiner.set_params)(\*\*params)      | Set the parameters of this estimator.                               |
| [`transform`](#skrub.InterpolationJoiner.transform)(X)                 | Transform a table by joining inferred values to it.                 |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit estimators to the `aux_table` provided during initialization.

`X` and `y` are mostly for scikit-learn compatibility.

* **Parameters:**
  **X**
  : The main table to which `self.aux_table` could be joined. If `X`
    is not `None`, an error is raised if any of the matching columns
    listed in `self.main_key` (or `self.key`) are missing from `X`.

  **y**
  : Ignored; only exists for compatibility with scikit-learn.
* **Returns:**
  InterpolationJoiner
  : Fitted [`InterpolationJoiner`](#skrub.InterpolationJoiner) instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None, \*\*fit_params)

Fit to data, then transform it.

Fits transformer to `X` and `y` with optional parameters `fit_params`
and returns a transformed version of `X`.

* **Parameters:**
  **X**
  : Input samples.

  **y**
  : Target values (None for unsupervised transformations).

  **\*\*fit_params**
  : Additional fit parameters.
    Pass only if the estimator accepts additional params in its `fit` method.
* **Returns:**
  **X_new**
  : Transformed array.

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

Transform a table by joining inferred values to it.

The values of the `main_key` columns in `X` (the main table) are used
to predict likely values for the contents of a matching row in
`aux_table` (the auxiliary table).

* **Parameters:**
  **X**
  : The (main) table to transform.
* **Returns:**
  DataFrame
  : The result of the join between `X` and inferred rows from `aux_table`.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="We illustrate the InterpolationJoiner, which is a type of join where values from the second table are inferred with machine-learning, rather than looked up in the table. It is useful when exact matches are not available but we have rows that are close enough to make an educated guess -- in this sense it is a generalization of a fuzzy_join.">  <div class="sphx-glr-thumbnail-title">Interpolation join: infer missing rows when joining two tables</div>
</div>
<!-- thumbnail-parent-div-close --></div>
