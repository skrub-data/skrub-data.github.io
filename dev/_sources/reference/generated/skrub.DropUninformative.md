# DropUninformative

### *class* skrub.DropUninformative(drop_if_constant=False, drop_if_unique=False, drop_null_fraction=1.0)

Drop column if it is found to be uninformative according to various criteria.

Columns are considered “uninformative” if the fraction of missing values is larger
than a threshold, if they contain one unique value, or if all values are unique.

* **Parameters:**
  **drop_if_constant**
  : If True, drop the column if it contains only one unique value. Missing values
    count as one additional distinct value.

  **drop_null_fraction**
  : Drop columns with a fraction of missing values larger than threshold. If None,
    keep the column even if all its values are missing.

  **drop_if_unique**
  : If True, drop the column if all values are unique (i.e. the
    number of unique values is equal to the number of rows). Missing values
    do not count as unique values for this criterion.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0.
    <br/>
    This functionality can drop informative columns and is unlikely to be
    of use in practice. It is therefore deprecated and will be removed in a
    future version.

#### SEE ALSO
[`Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: A full-frame transformer (as opposed to single column) that can drop columns with missing values.

[`DropCols`](skrub.DropColshtml.md#skrub.DropCols)
: Dropping cols by name, dtypes, or general skrub selectors.

`DropSimilar`
: Drops columns too closely correlated to the dataframe’s other columns.

### Notes

A column is considered to be “uninformative” if one or more of the following
issues are found:

- The fraction of missing values is larger than a certain fraction (by default,
  all values must be null for the column to be dropped).
- The column includes only one unique value (the column is constant). Missing
  values are considered a separate value.

### Examples

```pycon
>>> from skrub import DropUninformative
>>> import pandas as pd
>>> df = pd.DataFrame({"col1": [None, None, None]})
```

By default, only null columns are dropped:

```pycon
>>> du = DropUninformative()
>>> du.fit_transform(df["col1"])
[]
```

It is also possible to drop constant columns, or specify a lower null fraction
threshold:

```pycon
>>> df = pd.DataFrame({"col1": [1, 2, None], "col2": ["const", "const", "const"]})
>>> du = DropUninformative(drop_if_constant=True, drop_null_fraction=0.1)
>>> du.fit_transform(df["col1"])
[]
>>> du.fit_transform(df["col2"])
[]
```

### Methods

| [`fit`](#skrub.DropUninformative.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.DropUninformative.fit_transform)(column[, y])                      | Fit the encoder and transform a column.                                                |
| [`get_feature_names_out`](#skrub.DropUninformative.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.DropUninformative.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.DropUninformative.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.DropUninformative.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.DropUninformative.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.DropUninformative.transform)(column)                                   | Transform a column.                                                                    |
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
  : The input column to check.

  **y**
  : Ignored.
* **Returns:**
  column
  : The input column, or an empty list if the column is chosen to be
    dropped.

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
  : The input column to check.
* **Returns:**
  column
  : The input column, or an empty list if the column is chosen to be
    dropped.

<!-- !! processed by numpydoc !! -->
