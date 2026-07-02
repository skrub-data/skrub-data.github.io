# ToFloat

### *class* skrub.ToFloat(decimal='.', thousand=None, parentheses=False)

Convert a column to 32-bit floating-point numbers.

No conversion is attempted if the column has a datetime or categorical
dtype; a `RejectColumn` exception is raised.

Otherwise, we attempt to convert the column to float32. If the conversion
fails the column is rejected (a `RejectColumn` exception is raised).

For pandas, the output is always `np.float32`, not the extension dtype
`pd.Float64Dtype`. We do this conversion because most scikit-learn
estimators cannot handle those dtypes correctly yet, especially in the
presence of missing values (represented by `pd.NA` in such columns).

During `transform`, entries for which conversion fails are replaced by
null values.

* **Parameters:**
  **decimal**
  : Character to recognize as the decimal separator when converting from
    strings to floats. Other possible decimal separators are removed from
    the strings before conversion.

  **thousand**
  : Character used as thousands separator. Supported values are `"."`,
    `,`, space (`" "`), apostrophe (`"'"`), or `None` (no thousands
    separator). The decimal and thousands separators must differ.

  **parentheses**
  : Whether to recognize negative numbers represented using parentheses, e.g.,
    “(123.45)” → “-123.45”. If True, parentheses around numbers are replaced
    with a leading minus sign before conversion.

### Notes

This transformer does not validate number formatting. It simply removes
thousands separators and replaces the decimal separator. As a result,
some malformed inputs may still be converted but yield unexpected values.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub._to_float import ToFloat
```

A column that does not contain floats is converted if possible:

```pycon
>>> s = pd.Series(['1.1', None, '3.3'], name='x')
>>> s
0     1.1
1    ...
2     3.3
Name: x, dtype: ...
>>> s[0]
'1.1'
>>> to_float = ToFloat()
>>> float_s = to_float.fit_transform(s)
>>> float_s
0    1.1
1    NaN
2    3.3
Name: x, dtype: float32
>>> float_s[0]
np.float32(1.1)
```

A numeric column will also be converted to floats:

```pycon
>>> s = pd.Series([1, 2, 3])
>>> s
0    1
1    2
2    3
dtype: int64
>>> to_float.fit_transform(s)
0    1.0
1    2.0
2    3.0
dtype: float32
```

Boolean columns are treated as numbers:

```pycon
>>> s = pd.Series([True, False], name='b')
>>> s
0     True
1    False
Name: b, dtype: bool
>>> to_float.fit_transform(s)
0    1.0
1    0.0
Name: b, dtype: float32
```

```pycon
>>> s = pd.Series([True, None], name='b', dtype='boolean')
>>> s
0    True
1    <NA>
Name: b, dtype: boolean
>>> to_float.fit_transform(s)
0    1.0
1    NaN
Name: b, dtype: float32
>>> s = pd.Series([True, None], name='b')
>>> s
0    True
1    None
Name: b, dtype: object
>>> to_float.fit_transform(s)
0    1.0
1    NaN
Name: b, dtype: float32
```

float64 columns are converted to float32:

```pycon
>>> s = pd.Series([1.1, 2.2])
>>> s
0    1.1
1    2.2
dtype: float64
>>> to_float.fit_transform(s)
0    1.1
1    2.2
dtype: float32
```

Float64Dtype and Float32Dtype are cast to `np.float32`. We do this because
most scikit-learn estimators cannot handle `pd.Float32Dtype` correctly
yet, especially in the presence of missing values (represented by `pd.NA`
in such columns).

```pycon
>>> s = pd.Series([1.1, 2.2, None], dtype='Float32')
>>> s
0     1.1
1     2.2
2    <NA>
dtype: Float32
>>> to_float.fit_transform(s)
0    1.1
1    2.2
2    NaN
dtype: float32
```

Notice that `pd.NA` has been replaced by `np.nan`.

Columns that cannot be cast to numbers are rejected:

```pycon
>>> s = pd.Series(['1.1', '2.2', 'hello'], name='x')
>>> to_float.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Could not convert column 'x' to numbers.
```

Once a column has been accepted, all calls to `transform` will result in
the same output dtype. Values that fail to be converted become null values.

```pycon
>>> to_float = ToFloat().fit(pd.Series([1, 2]))
>>> to_float.transform(pd.Series(['3.3', 'hello']))
0    3.3
1    NaN
dtype: float32
```

Categorical and datetime columns are always rejected:

```pycon
>>> s = pd.Series(['1.1', '2.2'], dtype='category', name='s')
>>> s
0    1.1
1    2.2
Name: s, dtype: category
Categories (2, ...): ['1.1', '2.2']
>>> to_float.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Refusing to cast column 's' with dtype 'category' to numbers.
>>> to_float.fit_transform(pd.to_datetime(pd.Series(['2024-05-13'], name='s')))
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Refusing to cast column 's' with dtype 'datetime64[...]' to numbers.
```

float32 columns are passed through:

```pycon
>>> s = pd.Series([1.1, None], dtype='float32')
>>> to_float.fit_transform(s) is s
True
```

If `parentheses=True`, negative numbers represented using parentheses are converted

```pycon
>>> s = pd.Series(["-1,234.56", "1,234.56", "(1,234.56)"], name='parens')
>>> ToFloat(decimal=".", thousand=",", parentheses=True).fit_transform(s)
0   -1234.5...
1    1234.5...
2   -1234.5...
dtype: float32
```

Numbers that use scientific notation are converted:

```pycon
>>> s = pd.Series(["1.23e+4", "1.23E+4"], name="x")
>>> ToFloat(decimal=".").fit_transform(s)
0    12300.0
1    12300.0
Name: x, dtype: float32
```

It is possible to specify the thousands separator, e.g., to use `" "`

```pycon
>>> s = pd.Series(["4 567,89", "12 567,89"], name="x")
>>> ToFloat(decimal=",", thousand=" ").fit_transform(s)
0    4567.8...
1    12567.8...
Name: x, dtype: float32
```

This transformer does not do any advanced processing: it just replaces thousands
with `''` (the empty string) and replaces the decimal separator with `'.'`,
then tries to parse the result as a float.
Extra separators, mixed separators, or multiple grouping separators may lead to
unexpected or unwanted results:

```pycon
>>> s = pd.Series(["1,,234", "1,2,3,4.0"], name="x")
>>> ToFloat(decimal=".", thousand=",").fit_transform(s)
0    1234.0
1    1234.0
Name: x, dtype: float32
```

### Methods

| [`fit`](#skrub.ToFloat.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.ToFloat.fit_transform)(column[, y])                      | Fit the encoder and transform a column.                                                |
| [`get_feature_names_out`](#skrub.ToFloat.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.ToFloat.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.ToFloat.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.ToFloat.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.ToFloat.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.ToFloat.transform)(column)                                   | Transform a column.                                                                    |
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
  : The input transformed to Float32.

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
  : The input transformed to Float32.

<!-- !! processed by numpydoc !! -->
