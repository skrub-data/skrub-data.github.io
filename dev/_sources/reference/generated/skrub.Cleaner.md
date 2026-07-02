# Cleaner

### *class* skrub.Cleaner(drop_null_fraction=1.0, drop_if_constant=False, drop_if_unique=False, datetime_format=None, null_strings=None, parse_numbers=False, cast_to_float32=False, cast_to_str=False, n_jobs=1, numeric_dtype=None)

Column-wise consistency checks and sanitization of dtypes, null values and dates.

The `Cleaner` performs some consistency checks and basic preprocessing
such as detecting null values represented as strings (e.g. `'N/A'`), parsing
dates, and removing uninformative columns. See the “Notes” section for a full list.

* **Parameters:**
  **drop_null_fraction**
  : Fraction of null above which the column is dropped. If `drop_null_fraction`
    is set to `1.0`, the column is dropped if it contains only
    nulls or NaNs (this is the default behavior). If `drop_null_fraction` is a
    number in `[0.0, 1.0)`, the column is dropped if the fraction of nulls
    is strictly larger than `drop_null_fraction`. If `drop_null_fraction` is
    `None`, this selection is disabled: no columns are dropped based on the
    number of null values they contain.

  **drop_if_constant**
  : If set to true, drop columns that contain a single unique value. Note that
    missing values are considered as one additional distinct value.

  **drop_if_unique**
  : If set to true, drop columns that contain only unique values, i.e., the number
    of unique values is equal to the number of rows in the column. Numeric columns
    are never dropped.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0.
    <br/>
    This functionality can drop informative columns and is unlikely to be
    of use in practice. It is therefore deprecated and will be removed in a
    future version.

  **datetime_format**
  : The format to use when parsing dates. If None, the format is inferred.

  **parse_numbers**
  : Whether to parse strings that represent numeric values.
    - `False`: no numeric parsing is attempted.
    - `True`: apply [`ToFloat`](skrub.ToFloathtml.md#skrub.ToFloat) to string columns. String columns
      whose non-missing values can all be parsed as numbers are converted to
      `float32`.

  **cast_to_float32**
  : Whether to cast numeric columns to `float32`.
    If set to `True`, numeric columns are converted to `float32`.

  **cast_to_str**
  : If `True`, apply the `ToStr` transformer to non-numeric,
    non-categorical, and non-datetime columns, converting them to strings.
    If `False`, this step is skipped and such columns retain their
    original dtype (e.g., lists, structs).

  **numeric_dtype**
  : If set to “float32”, this parameter has the same effect as
    `cast_to_float32=True` and `parse_numbers=True`: it casts
    numeric columns to `float32`.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0: Use `cast_to_float32=True` with `parse_numbers=True` instead.

  **null_strings**
  : Additional strings to consider as null values, beyond the default list.

  **n_jobs**
  : Number of jobs to run in parallel.
    `None` means 1 unless in a joblib `parallel_backend` context.
    `-1` means using all processors.
* **Attributes:**
  **all_processing_steps_**
  : Maps the name of each column to a list of all the processing steps that were
    applied to it.

  **all_outputs_**
  : Column names of the output of `transform`.

#### SEE ALSO
[`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer)
: Process columns of a dataframe and convert them to a numeric (vectorized) representation.

[`ToFloat`](skrub.ToFloathtml.md#skrub.ToFloat)
: Convert numeric columns to `np.float32`, to have consistent numeric types and representation of missing values. More informative columns (e.g., categorical or datetime) are not converted.

[`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
: Apply a given transformer to each column in a selection of columns. Useful to complement the default heuristics of the `Cleaner`.

[`DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative)
: Drop columns that are considered uninformative, e.g., containing only null values or a single unique value.

### Notes

The `Cleaner` performs the following set of transformations on each column:

- `CleanNullStrings()`: replace strings used to represent missing values
  with NA markers.
- [`DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative): drop the column if it is considered to be
  “uninformative”. A column is considered to be “uninformative” if it contains
  only missing values (`drop_null_fraction`) or only a constant value
  (`drop_if_constant`).
  By default, the `Cleaner` keeps all columns, unless they contain only
  missing values.
- [`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime): parse datetimes represented as strings and return them as
  actual datetimes with the correct dtype. If `datetime_format` is provided,
  it is forwarded to [`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime). Otherwise, the format is inferred.
- [`ToFloat`](skrub.ToFloathtml.md#skrub.ToFloat):
  - if `parse_numbers=True`, apply [`ToFloat`](skrub.ToFloathtml.md#skrub.ToFloat) on string columns,
    converting strings whose non-missing values can all be parsed as numbers
    to `float32`;
  - if `cast_to_float32=True`, apply [`ToFloat`](skrub.ToFloathtml.md#skrub.ToFloat) on numeric
    columns to cast them to `float32`.
- `CleanCategories()`: process categorical columns depending on the dataframe
  library (Pandas or Polars) to force consistent typing and avoid issues downstream.
- `ToStr()`: convert columns to strings unless they are numerical,
  categorical, or datetime. This step is controlled by the `cast_to_str`
  parameter. When `cast_to_str=False` (default), string conversion is
  skipped. When `cast_to_str=True`, string conversion is applied.

### Examples

```pycon
>>> from skrub import Cleaner
>>> import pandas as pd
>>> df = pd.DataFrame({"num_str": ["1", "2"], "num": [1, 2], "f": [1.0, 2.0]})
>>> Cleaner(parse_numbers=False).fit_transform(df).dtypes
num_str    ...
num        ...
f          float64
dtype: object
>>> cleaner = Cleaner(parse_numbers=True, cast_to_float32=True)
>>> cleaner.fit_transform(df).dtypes
num_str    float32
num        ...
f          float32
dtype: object
>>> df = pd.DataFrame({
...     'A': ['one', 'two', 'two', 'three'],
...     'B': ['02/02/2024', '23/02/2024', '12/03/2024', '13/03/2024'],
...     'C': ['1.5', 'N/A', '12.2', 'N/A'],
...     'D': [1.5, 2.0, 2.5, 3.0],
... })
>>> df
       A           B     C    D
0    one  02/02/2024   1.5  1.5
1    two  23/02/2024   N/A  2.0
2    two  12/03/2024  12.2  2.5
3  three  13/03/2024   N/A  3.0
>>> df.dtypes
A       ...
B       ...
C       ...
D   float64
dtype: object
```

The Cleaner will parse datetime columns and convert nulls to dtypes
suitable to those of the column (e.g., `np.NaN` for numerical columns).

```pycon
>>> cleaner = Cleaner()
>>> cleaner.fit_transform(df)
       A          B     C    D
0    one 2024-02-02   1.5  1.5
1    two 2024-02-23  ...  2.0
2    two 2024-03-12  12.2  2.5
3  three 2024-03-13  ...  3.0
```

```pycon
>>> cleaner.fit_transform(df).dtypes
A               ...
B    datetime64[ns]
C               ...
D           float64
dtype: object
```

Columns can be excluded from processing by combining the `Cleaner` with
[`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols). For example, to exclude the datetime column from
processing and keep it as a string, we can do:

```pycon
>>> from skrub import ApplyToCols
>>> import skrub.selectors as s
>>> ApplyToCols(Cleaner(), s.all() - 'B').fit_transform(df)
            B      A     C    D
0  02/02/2024    one   1.5  1.5
1  23/02/2024    two   ...  2.0
2  12/03/2024    two  12.2  2.5
3  13/03/2024  three   ...  3.0
```

We can inspect all the processing steps that were applied to a given column:

```pycon
>>> cleaner.all_processing_steps_['A']
[CleanNullStrings(), DropUninformative()]
>>> cleaner.all_processing_steps_['B']
[CleanNullStrings(), DropUninformative(), ToDatetime()]
>>> cleaner.all_processing_steps_['C']
[CleanNullStrings(), DropUninformative()]
>>> cleaner.all_processing_steps_['D']
[DropUninformative()]
```

### Methods

| [`fit`](#skrub.Cleaner.fit)(X[, y])                                               | Fit transformer.                                                           |
|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| [`fit_transform`](#skrub.Cleaner.fit_transform)(X[, y])                           | Fit transformer and transform dataframe.                                   |
| [`get_feature_names_out`](#skrub.Cleaner.get_feature_names_out)([input_features]) | Return the column names of the output of `transform` as a list of strings. |
| [`get_params`](#skrub.Cleaner.get_params)([deep])                                 | Get parameters for this estimator.                                         |
| [`set_output`](#skrub.Cleaner.set_output)(\*[, transform])                        | Set output container.                                                      |
| [`set_params`](#skrub.Cleaner.set_params)(\*\*params)                             | Set the parameters of this estimator.                                      |
| [`transform`](#skrub.Cleaner.transform)(X)                                        | Transform dataframe.                                                       |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit transformer.

* **Parameters:**
  **X**
  : Input data to transform.

  **y**
  : Target values for supervised learning (None for unsupervised
    transformations).
* **Returns:**
  **self**
  : The fitted estimator.

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

Fit transformer and transform dataframe.

* **Parameters:**
  **X**
  : Input data to transform.

  **y**
  : Target values for supervised learning (None for unsupervised
    transformations).
* **Returns:**
  dataframe
  : The transformed input.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Return the column names of the output of `transform` as a list of strings.

* **Parameters:**
  **input_features**
  : Ignored.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of strings
  : The column names.

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

Transform dataframe.

* **Parameters:**
  **X**
  : Input data to transform.
* **Returns:**
  dataframe
  : The transformed input.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div>
<!-- thumbnail-parent-div-close --></div>
