# ToCategorical

### *class* skrub.ToCategorical

Convert a string column to Categorical dtype.

This transformer ensures that a given string or categorical column has
Categorical dtype. This is done to mark columns to be treated as categorical
by downstream transformers and learners.

### Notes

The main benefit of converting columns to categorical is that categorical
columns can be recognized by
scikit-learn’s `HistGradientBoostingRegressor` and
`HistGradientBoostingClassifier` with their
`categorical_features='from_dtype'` option. This transformer is therefore
particularly useful as the `low_cardinality_transformer` parameter of the
`TableVectorizer` when combined with one of those supervised learners.

A pandas column with dtype `string` or `object` containing strings, or
a polars column with dtype `String`, is converted to a categorical
column. Categorical columns are passed through.

Any other type of column is rejected by raising a `RejectColumn`
exception. **Note:** the `TableVectorizer` only sends string or
categorical columns to its `low_cardinality_transformer`. Therefore it is
always safe to use a `ToCategorical` instance as the
`low_cardinality_transformer`.

The output of `transform` also always has a Categorical dtype. The categories
are not necessarily the same across different calls to `transform`. Indeed,
scikit-learn estimators do not inspect the dtype’s categories but the actual
values. Converting to a Categorical is therefore just a way to mark a
column and indicate to downstream estimators that this column should be treated
as categorical. Ensuring they are encoded consistently, handling unseen
categories at test time, etc. is the responsibility of encoders such as
`OneHotEncoder` and `LabelEncoder`, or of estimators that handle categories
themselves such as `HistGradientBoostingRegressor`.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import ToCategorical
```

A string column is converted to a categorical column.

```pycon
>>> s = pd.Series(['one', 'two', None], name='c')
>>> s
0     one
1     two
2     ...
Name: c, dtype: ...
>>> to_cat = ToCategorical()
>>> to_cat.fit_transform(s)
0    one
1    two
2    ...
Name: c, dtype: ...
Categories (2, ...): ['one', 'two']
```

The dtypes (the list of categories) of the outputs of `transform` may
vary. This transformer only ensures the dtype is Categorical to mark the
column as such for downstream encoders which will perform the actual
encoding.

```pycon
>>> s = pd.Series(['four', 'five'], name='c')
>>> to_cat.transform(s)
0    four
1    five
Name: c, dtype: category
Categories (2, ...): ['five', 'four']
```

Columns that already have a Categorical dtype are passed through:

```pycon
>>> s = pd.Series(['one', 'two'], name='c', dtype='category')
>>> to_cat.fit_transform(s) is s
True
```

Columns that are not strings nor categorical are rejected:

```pycon
>>> to_cat.fit_transform(pd.Series([1.1, 2.2], name='c'))
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'c' does not contain strings.
```

`object` columns that do not contain only strings are also rejected:

```pycon
>>> s = pd.Series(['one', 1], name='c')
>>> to_cat.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'c' does not contain strings.
```

No special handling of `StringDtype` vs `object` columns is done, the
behavior is the same as `pd.astype('category')`: if the input uses the
extension dtype, the categories of the output will, too.

```pycon
>>> s = pd.Series(['cat A', 'cat B', None], name='c', dtype='string')
>>> s
0    cat A
1    cat B
2     <NA>
Name: c, dtype: string
>>> to_cat.fit_transform(s)
0    cat A
1    cat B
2     <NA>
Name: c, dtype: category
Categories (2, string): [...]
>>> _.cat.categories.dtype
```

Polars string columns are converted to the `Categorical` dtype (not `Enum`). As
for pandas, categories may vary across calls to `transform`.

```pycon
>>> import pytest
>>> pl = pytest.importorskip("polars")
>>> s = pl.Series('c', ['one', 'two', None])
>>> to_cat.fit_transform(s)
shape: (3,)
Series: 'c' [cat]
[
    "one"
    "two"
    null
]
```

Polars Categorical or Enum columns are passed through:

```pycon
>>> s = pl.Series('c', ['one', 'two'], dtype=pl.Enum(['one', 'two', 'three']))
>>> s
shape: (2,)
Series: 'c' [enum]
[
    "one"
    "two"
]
>>> to_cat.fit_transform(s) is s
True
```

### Methods

| [`fit`](#skrub.ToCategorical.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.ToCategorical.fit_transform)(column[, y])                      | Fit the encoder and transform a column.                                                |
| [`get_feature_names_out`](#skrub.ToCategorical.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.ToCategorical.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.ToCategorical.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.ToCategorical.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.ToCategorical.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.ToCategorical.transform)(column)                                   | Transform a column.                                                                    |
<!-- !! processed by numpydoc !! -->

#### fit(column, y=None, \*\*kwargs)

Fit the transformer.

This default implementation simply calls `fit_transform()` and
returns `self`.

Subclasses should implement `fit_transform` and `transform`.

* **Parameters:**
  **column**
  : Unlike most scikit-learn transformers, single-column transformers
    transform a single column, not a whole dataframe.

  **y**
  : Prediction targets.

  **\*\*kwargs**
  : Extra named arguments are passed to `self.fit_transform()`.
* **Returns:**
  self
  : The fitted transformer.

<!-- !! processed by numpydoc !! -->

#### fit_transform(column, y=None)

Fit the encoder and transform a column.

* **Parameters:**
  **column**
  : The input to transform.

  **y**
  : Ignored.
* **Returns:**
  **transformed**
  : The input transformed to Categorical.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Get the output feature names.

* **Parameters:**
  **input_features**
  : Input feature names. Ignored.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The names of the output features.

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

Default no-op implementation for set_output.

Skrub transformers already output dataframes of the correct type by
default so there is usually no need for set_output to do anything.

Subclasses are of course free to redefine set_output (e.g. by
inheriting from `TransformerMixin` before SingleColumnTransformer).

* **Parameters:**
  **transform**
  : Ignored.
* **Returns:**
  SingleColumnTransformer
  : Returns self.

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

#### set_transform_request(, column='$UNCHANGED$')

Configure whether metadata should be requested to be passed to the `transform` method.

Note that this method is only relevant when this estimator is used as a
sub-estimator within a [meta-estimator](https://scikit-learn.org/stable/glossary.html#term-meta-estimator) and metadata routing is enabled
with `enable_metadata_routing=True` (see [`sklearn.set_config()`](https://scikit-learn.org/stable/modules/generated/sklearn.set_config.html#sklearn.set_config)).
Please check the [User Guide](https://scikit-learn.org/stable/metadata_routing.html#metadata-routing) on how the routing
mechanism works.

The options for each parameter are:

- `True`: metadata is requested, and passed to `transform` if provided. The request is ignored if metadata is not provided.
- `False`: metadata is not requested and the meta-estimator will not pass it to `transform`.
- `None`: metadata is not requested, and the meta-estimator will raise an error if the user provides it.
- `str`: metadata should be passed to the meta-estimator with this given alias instead of the original name.

The default (`sklearn.utils.metadata_routing.UNCHANGED`) retains the
existing request. This allows you to change the request for some
parameters and not others.

#### Versionadded
Added in version 1.3.

* **Parameters:**
  **column**
  : Metadata routing for `column` parameter in `transform`.
* **Returns:**
  **self**
  : The updated object.

<!-- !! processed by numpydoc !! -->

#### transform(column)

Transform a column.

* **Parameters:**
  **column**
  : The input to transform.
* **Returns:**
  **transformed**
  : The input transformed to Categorical.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div>
<!-- thumbnail-parent-div-close --></div>
