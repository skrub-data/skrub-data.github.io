# TableVectorizer

### *class* skrub.TableVectorizer(, cardinality_threshold=40, low_cardinality=OneHotEncoder(drop='if_binary', dtype='float32', handle_unknown='ignore', sparse_output=False), high_cardinality=StringEncoder(), numeric=PassThrough(), datetime=DatetimeEncoder(), specific_transformers=(), drop_null_fraction=1.0, drop_if_constant=False, drop_if_unique=False, datetime_format=None, null_strings=None, n_jobs=None)

Transform a dataframe to a numeric (vectorized) representation.

This transformer preprocesses the given dataframe by first cleaning the data
to ensure consistent numerical dtypes (float32). The TableVectorizer will
automatically convert to float32 any string column that contains only numerical
information.
Then it encodes each column with an encoder suitable for its dtype. Categorical
features are encoded differently depending on their cardinality.

* **Parameters:**
  **cardinality_threshold**
  : String and categorical columns with a number of unique values strictly smaller
    than this threshold are handled by the transformer `low_cardinality`, the rest
    are handled by the transformer `high_cardinality`.

  **low_cardinality**
  : The transformer for string or categorical columns with strictly fewer than
    `cardinality_threshold` unique values. By default, we use a
    [`OneHotEncoder`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) that ignores unknown categories
    and drop one of the transformed columns if the feature contains only 2
    categories.

  **high_cardinality**
  : The transformer for string or categorical columns with at least
    `cardinality_threshold` unique values. The default is a
    [`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder) with 30 components (30 output columns for each
    input).
    <br/>
    #### Versionchanged
    Changed in version 0.6.0: The default `high_cardinality` encoder has been changed from
    [`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder) to [`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder).

  **numeric**
  : The transformer for numeric columns (floats, ints, booleans).

  **datetime**
  : The transformer for date and datetime columns. By default, we use a
    [`DatetimeEncoder`](skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder).

  **specific_transformers**
  : Override the categories above for the given columns and force using the
    specified transformer. This disables any preprocessing usually done by
    the TableVectorizer; the columns are passed to the transformer without
    any modification. A column is not allowed to appear twice in
    `specific_transformers`.
    Consider wrapping the `TableVectorizer` in  [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
    to select or exclude specific columns from the processing. Alternatively,
    the [skrub Data Ops](../../data_opshtml.md#user-guide-data-ops-index) allows for more complex
    pre-processing.

  **drop_null_fraction**
  : Fraction of null above which the column is dropped. If `drop_null_fraction` is
    set to `1.0`, the column is dropped if it contains only
    nulls or NaNs (this is the default behavior). If `drop_null_fraction` is a
    number in `[0.0, 1.0)`, the column is dropped if the fraction of nulls
    is strictly larger than `drop_null_fraction`. If `drop_null_fraction` is `None`,
    this selection is disabled: no columns are dropped based on the number
    of null values they contain.

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

  **null_strings**
  : Additional strings to consider as null values, beyond the default list.

  **n_jobs**
  : Number of jobs to run in parallel.
    `None` means 1 unless in a joblib `parallel_backend` context.
    `-1` means using all processors.
* **Attributes:**
  **transformers_**
  : Maps the name of each column to the fitted transformer that was applied
    to it.

  **column_to_kind_**
  : Maps each column name to the kind (`"high_cardinality"`,
    `"low_cardinality"`, `"specific"`, etc.) it was assigned.

  **kind_to_columns_**
  : The reverse of `column_to_kind_`: maps each kind of column
    (`"high_cardinality"`, `"low_cardinality"`, etc.) to a list of
    column names. For example `kind_to_columns['datetime']` contains the
    names of all datetime columns.

  **input_to_outputs_**
  : Maps the name of each input column to the names of the corresponding
    output columns.

  **output_to_input_**
  : The reverse of `input_to_outputs_`: maps the name of each output
    column to the name of the column in the input dataframe from which it
    was derived.

  **all_processing_steps_**
  : Maps the name of each column to a list of all the processing steps that were
    applied to it. Those steps may include some pre-processing transformations such
    as converting strings to datetimes or numbers, the main transformer (e.g. the
    [`DatetimeEncoder`](skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder)), and a post-processing step casting the main
    transformer’s output to [`numpy.float32`](https://numpy.org/doc/stable/reference/arrays.scalars.html#numpy.float32). See the “Examples” section below
    for details.

  **feature_names_in_**
  : The names of the input columns, after applying some cleaning (casting
    all column names to strings and deduplication).

  **n_features_in_**
  : The number of input columns.

  **all_outputs_**
  : The names of the output columns.

#### SEE ALSO
[`tabular_pipeline`](skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline)
: A function that accepts a scikit-learn estimator and creates a pipeline combining a `TableVectorizer`, optional missing value imputation and the provided estimator.

[`Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: Preprocesses each column of a dataframe with consistency checks and sanitization, e.g., of null values or dates.

[`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
: Apply a given transformer to each column in a selection of columns. Combine this transformer with the skrub selectors to select columns based on advanced rules.

[`DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative)
: Drop columns that are considered uninformative, e.g., containing only null values or a single unique value.

### Notes

The TableVectorizer applies a different transformation to each of several kinds of columns:

- `numeric`: floats, integers, and booleans.
- `datetime`: datetimes and dates.
- `low_cardinality`: string and categorical columns with a count
  of unique values smaller than a given threshold (40 by default). Category encoding
  schemes such as one-hot encoding, ordinal encoding etc. are typically appropriate
  for columns with few unique values.
- `high_cardinality`: string and categorical columns with many
  unique values, such as free-form text. Such columns have so many distinct values
  that it is not possible to assign a distinct representation to each: the dimension
  would be too large and there would be too few examples of each category.
  Representations designed for text, such as topic modelling
  ([`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder)) or locality-sensitive hashing
  (`MinHash`) are more appropriate.

#### NOTE
Transformations are applied **independently on each column**. A
different transformer instance is used for each column separately;
multivariate transformations are therefore not supported.

The transformer for each kind of column can be configured with the corresponding
parameter. A transformer is expected to be a [compatible scikit-learn transformer](https://scikit-learn.org/stable/glossary.html#term-transformer). Special-cased
strings `"drop"` and `"passthrough"` are accepted as well, to indicate to drop
the columns or to pass them through untransformed, respectively.

Additionally, it is possible to specify transformers for specific columns,
overriding the categorization described above. This is done by providing a
list of pairs `(transformer, list_of_columns)` as the
`specific_transformers` parameter.

### Examples

```pycon
>>> from skrub import TableVectorizer
>>> import pandas as pd
>>> df = pd.DataFrame({
...     'A': ['one', 'two', 'two', 'three'],
...     'B': ['02/02/2024', '23/02/2024', '12/03/2024', '13/03/2024'],
...     'C': ['1.5', 'N/A', '12.2', 'N/A'],
... })
>>> df
       A           B     C
0    one  02/02/2024   1.5
1    two  23/02/2024   N/A
2    two  12/03/2024  12.2
3  three  13/03/2024   N/A
>>> df.dtypes
A         ...
B         ...
C         ...
dtype: object
```

```pycon
>>> vectorizer = TableVectorizer()
>>> vectorizer.fit_transform(df)
   A_one  A_three  A_two  B_year  B_month  B_day  B_total_seconds     C
0    1.0      0.0    0.0  2024.0      2.0    2.0     1.706832e+09   1.5
1    0.0      0.0    1.0  2024.0      2.0   23.0     1.708646e+09   NaN
2    0.0      0.0    1.0  2024.0      3.0   12.0     1.710202e+09  12.2
3    0.0      1.0    0.0  2024.0      3.0   13.0     1.710288e+09   NaN
```

We can inspect which outputs were created from a given column in the input
dataframe:

```pycon
>>> vectorizer.input_to_outputs_['B']
['B_year', 'B_month', 'B_day', 'B_total_seconds']
```

and the reverse mapping:

```pycon
>>> vectorizer.output_to_input_['B_total_seconds']
'B'
```

We can also see the encoder that was applied to a given column:

```pycon
>>> vectorizer.transformers_['B']
DatetimeEncoder()
>>> vectorizer.transformers_['A']
OneHotEncoder(drop='if_binary', dtype='float32', handle_unknown='ignore',
              sparse_output=False)
>>> vectorizer.transformers_['A'].categories_
[array(['one', 'three', 'two'], dtype=...)]
```

We can see the columns grouped by the kind of encoder that was applied
to them:

```pycon
>>> vectorizer.kind_to_columns_
{'numeric': ['C'], 'datetime': ['B'], 'low_cardinality': ['A'], 'high_cardinality': [], 'specific': []}
```

As well as the reverse mapping (from each column to its kind):

```pycon
>>> vectorizer.column_to_kind_
{'C': 'numeric', 'B': 'datetime', 'A': 'low_cardinality'}
```

Before applying the main transformer, the `TableVectorizer` applies
several preprocessing steps, for example to detect numbers or dates that are
represented as strings. By default, columns that contain only null values are
dropped. Moreover, a final post-processing step is applied to all
non-categorical columns in the encoder’s output to cast them to float32.
If `datetime_format` is provided, it will be used to parse all datetime
columns.

We can inspect all the processing steps that were applied to a given column:

```pycon
>>> vectorizer.all_processing_steps_['B']
[CleanNullStrings(), DropUninformative(), ToDatetime(), DatetimeEncoder(), {'B_day': ToFloat(), 'B_month': ToFloat(), ...}]
```

Note that as the encoder (`DatetimeEncoder()` above) produces multiple
columns, the last processing step is not described by a single transformer
like the previous ones but by a mapping from column name to transformer.

`all_processing_steps_` is useful to inspect the details of the
choices made by the `TableVectorizer` during preprocessing, for example:

```pycon
>>> vectorizer.all_processing_steps_['B'][2]
ToDatetime()
>>> _.format_
'%d/%m/%Y'
```

**Transformers are applied separately to each column**

The `TableVectorizer` vectorizes each column separately – a different
transformer is applied to each column; multivariate transformers are not
allowed.

```pycon
>>> df_1 = pd.DataFrame(dict(A=['one', 'two'], B=['three', 'four']))
>>> vectorizer = TableVectorizer().fit(df_1)
>>> vectorizer.transformers_['A'] is not vectorizer.transformers_['B']
True
>>> vectorizer.transformers_['A'].categories_
[array(['one', 'two'], dtype=...)]
>>> vectorizer.transformers_['B'].categories_
[array(['four', 'three'], dtype=...)]
```

**Overriding the transformer for specific columns**

We can also provide transformers for specific columns. In that case the
provided transformer has full control over the associated columns; no other
processing is applied to those columns. A column cannot appear twice in the
`specific_transformers`.

The overrides are provided as a list of pairs:
`(transformer, list_of_column_names)`.

```pycon
>>> from sklearn.preprocessing import OrdinalEncoder
>>> vectorizer = TableVectorizer(
...     specific_transformers=[('drop', ['A']), (OrdinalEncoder(), ['B'])]
... )
>>> df
       A           B     C
0    one  02/02/2024   1.5
1    two  23/02/2024   N/A
2    two  12/03/2024  12.2
3  three  13/03/2024   N/A
>>> vectorizer.fit_transform(df)
     B     C
0  0.0   1.5
1  3.0   NaN
2  1.0  12.2
3  2.0   NaN
```

Here the column ‘A’ has been dropped and the column ‘B’ has been passed to
the `OrdinalEncoder` (instead of the default choice which would have been
`DatetimeEncoder`).

We can see that ‘A’ and ‘B’ are now treated as ‘specific’ columns:

```pycon
>>> vectorizer.column_to_kind_
{'C': 'numeric', 'A': 'specific', 'B': 'specific'}
```

Preprocessing and postprocessing steps are not applied to columns appearing
in `specific_columns`. For example ‘B’ has not gone through
`ToDatetime()`:

```pycon
>>> vectorizer.all_processing_steps_
{'A': [Drop()], 'B': [OrdinalEncoder()], 'C': [CleanNullStrings(), DropUninformative(), ToFloat(), PassThrough(), {'C': ToFloat()}]}
```

Specifying several `specific_transformers` for the same column is not allowed.

```pycon
>>> vectorizer = TableVectorizer(
...     specific_transformers=[('passthrough', ['A', 'B']), ('drop', ['A'])]
... )
```

```pycon
>>> vectorizer.fit_transform(df)
Traceback (most recent call last):
    ...
ValueError: Column 'A' used twice in 'specific_transformers', at indices 0 and 1.
```

### Methods

| [`fit`](#skrub.TableVectorizer.fit)(X[, y])                                               | Fit transformer.                                                           |
|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| [`fit_transform`](#skrub.TableVectorizer.fit_transform)(X[, y])                           | Fit transformer and transform dataframe.                                   |
| [`get_feature_names_out`](#skrub.TableVectorizer.get_feature_names_out)([input_features]) | Return the column names of the output of `transform` as a list of strings. |
| [`get_params`](#skrub.TableVectorizer.get_params)([deep])                                 | Get parameters for this estimator.                                         |
| [`set_output`](#skrub.TableVectorizer.set_output)(\*[, transform])                        | Set output container.                                                      |
| [`set_params`](#skrub.TableVectorizer.set_params)(\*\*params)                             | Set the parameters of this estimator.                                      |
| [`transform`](#skrub.TableVectorizer.transform)(X)                                        | Transform dataframe.                                                       |
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
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="The following example illustrates the use of the SquashingScaler, a transformer that can rescale and squash numerical features to a range that works well with neural networks and perhaps also other related models. Its basic idea is to rescale the features based on quantile statistics (to be robust to outliers), and then perform a smooth squashing function to limit the outputs to a pre-defined range. This transform has been found to even work well when applied to one-hot encoded features.">  <div class="sphx-glr-thumbnail-title">SquashingScaler: Robust numerical preprocessing for neural networks</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we illustrate how to better integrate datetime features in machine learning models with the |DatetimeEncoder|.">  <div class="sphx-glr-thumbnail-title">Handling datetime features with the DatetimeEncoder</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to use DataOp.skb.subsample to speed up interactive construction of a skrub DataOps plan by computing previews on a subsampled version of the original data.">  <div class="sphx-glr-thumbnail-title">Subsampling for faster development</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Joining tables may be difficult if one entry on one side does not have an exact match on the other side.">  <div class="sphx-glr-thumbnail-title">Spatial join for flight data: Joining across multiple columns</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
