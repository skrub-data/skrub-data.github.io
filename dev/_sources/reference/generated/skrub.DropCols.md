# DropCols

### *class* skrub.DropCols(cols)

Drop a subset of a DataFrame’s columns.

The other columns are kept in their original order. A `ValueError` is raised if
any of the provided column names are not in the dataframe.

Accepts [`pandas.DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame) and `polars.DataFrame` inputs.

* **Parameters:**
  **cols**
  : The columns to drop, or a selector. A single column name can be passed as a
    `str`: `"col_name"` is the same as `["col_name"]`. See the
    [selectors](../../modules/multi_column_operations/selectorshtml.md#user-guide-selectors) user guide for more info on selectors.

#### SEE ALSO
[`SelectCols`](skrub.SelectColshtml.md#skrub.SelectCols)
: Selecting cols by name, dtypes, or general skrub selectors.

[`Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: Can be used to drop columns with too many nulls (or NaNs).

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import DropCols
>>> df = pd.DataFrame({"A": [1, 2], "B": [10, 20], "C": ["x", "y"]})
>>> df
   A   B  C
0  1  10  x
1  2  20  y
>>> DropCols(["A", "C"]).fit_transform(df)
    B
0  10
1  20
>>> DropCols(["X"]).fit_transform(df)
Traceback (most recent call last):
    ...
ValueError: The following columns are requested for selection but missing from dataframe: ['X']
```

### Methods

| [`fit`](#skrub.DropCols.fit)(X[, y])                                               | Fit the transformer.                         |
|------------------------------------------------------------------------------------|----------------------------------------------|
| [`fit_transform`](#skrub.DropCols.fit_transform)(X[, y])                           | Fit to data, then transform it.              |
| [`get_feature_names_out`](#skrub.DropCols.get_feature_names_out)([input_features]) | Get output feature names for transformation. |
| [`get_params`](#skrub.DropCols.get_params)([deep])                                 | Get parameters for this estimator.           |
| [`set_output`](#skrub.DropCols.set_output)(\*[, transform])                        | Set output container.                        |
| [`set_params`](#skrub.DropCols.set_params)(\*\*params)                             | Set the parameters of this estimator.        |
| [`transform`](#skrub.DropCols.transform)(X)                                        | Transform a dataframe by dropping columns.   |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit the transformer.

* **Parameters:**
  **X**
  : If `X` is a DataFrame, the transformer checks that all the column
    names provided in `self.cols` can be found in `X`.

  **y**
  : Unused.
* **Returns:**
  DropCols
  : The transformer itself.

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

#### get_feature_names_out(input_features=None)

Get output feature names for transformation.

* **Parameters:**
  **input_features**
  : Ignored.
* **Returns:**
  **feature_names_out**
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

Transform a dataframe by dropping columns.

* **Parameters:**
  **X**
  : The DataFrame on which to apply the selection.
* **Returns:**
  DataFrame
  : The input DataFrame `X` after dropping the columns listed in
    `self.cols`.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
