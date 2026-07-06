# DurationToFloat

### *class* skrub.DurationToFloat

Convert duration columns to seconds.

This transformer converts duration columns to seconds. It only works on
columns with duration dtype. Other dtypes are rejected.

### Examples

```pycon
>>> import pandas as pd
>>> from datetime import timedelta
>>> from skrub import DurationToFloat
>>> s = pd.Series([
...     timedelta(seconds=3600), timedelta(minutes=2), timedelta(days=1)
... ])
>>> s
0   0 days 01:00:00
1   0 days 00:02:00
2   1 days 00:00:00
dtype: timedelta64[...]
>>> converter = DurationToFloat()
>>> converter.fit_transform(s)
0    3600.0
1    120.0
2    86400.0
dtype: float32
```

Columns that do not have `duration` dtype are rejected:

```pycon
>>> s = pd.Series(['1 day', '2 days', '3 days'], name='s')
>>> converter.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Expected a duration column, got...
```

For `polars`, only columns with `Duration` dtype are modified:

```pycon
>>> import pytest
>>> pl = pytest.importorskip('polars')
>>> s = pl.Series([
...     timedelta(seconds=3600), timedelta(minutes=2), timedelta(days=1)
... ])
>>> s
shape: (3,)
Series: '' [duration[μs]]
[
    1h
    2m
    1d
]
>>> converter.fit_transform(s)
shape: (3,)
Series: '' [f32]
[
    3600.0
    120.0
    86400.0
]
>>> s = pl.Series('s', ['1 day', '2 days', '3 days'], dtype=pl.String)
>>> converter.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Expected a duration column, got String
```

### Methods

| [`fit`](#skrub.DurationToFloat.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`get_feature_names_out`](#skrub.DurationToFloat.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.DurationToFloat.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.DurationToFloat.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.DurationToFloat.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.DurationToFloat.set_transform_request)(\*[, col])        | Configure whether metadata should be requested to be passed to the `transform` method. |

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

#### set_transform_request(, col='$UNCHANGED$')

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
  **col**
  : Metadata routing for `col` parameter in `transform`.
* **Returns:**
  **self**
  : The updated object.

<!-- !! processed by numpydoc !! -->
