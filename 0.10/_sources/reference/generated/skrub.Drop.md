# Drop

### *class* skrub.Drop

Drop the selected DataFrame’s column unconditionally.

The other columns are kept in their original order. A `ValueError` is raised if
any of the provided column names are not in the dataframe.
This transformer is different from [`DropCols`](skrub.DropColshtml.md#skrub.DropCols) in that it is designed
to be used with other transformers, for example to remove all columns with a
given type from a transformation.

* **Parameters:**
  **cols**
  : The columns to drop, or a selector. A single column name can be passed as a
    `str`: `"col_name"` is the same as `["col_name"]`. See the
    [selectors](../../modules/multi_column_operations/selectorshtml.md#user-guide-selectors) user guide for more info on selectors.

#### SEE ALSO
[`DropCols`](skrub.DropColshtml.md#skrub.DropCols)
: drop columns by name, or skrub selectors.

[`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
: Can be used to Apply a transformer to selected columns in a dataframe.

[`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer)
: Transform a dataframe to a numeric (vectorized) representation.

### Examples

`Drop` is meant to be used with other transformers to drop columns with a
given type or other property.

For example, if we want to vectorize a dataframe but drop the numeric columns,
we can do:

```pycon
>>> import pandas as pd
>>> from skrub import Drop
>>> df = pd.DataFrame({"num": [1,2,3], "text": ["hello", "world", "foo"]})
>>> df
   num  text
0    1  hello
1    2  world
2    3    foo
>>> from skrub import TableVectorizer
>>> TableVectorizer(numeric=Drop()).fit_transform(df)
    text_foo  text_hello  text_world
0       0.0         1.0         0.0
1       0.0         0.0         1.0
2       1.0         0.0         0.0
```

Here, only the “text” column is vectorized, and the “num” column is dropped
entirely.

### Methods

| [`fit`](#skrub.Drop.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`get_feature_names_out`](#skrub.Drop.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.Drop.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.Drop.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.Drop.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.Drop.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |

| **fit_transform**   |    |
|---------------------|----|
| **transform**       |    |
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
