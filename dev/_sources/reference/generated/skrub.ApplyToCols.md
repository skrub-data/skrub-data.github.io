# ApplyToCols

### *class* skrub.ApplyToCols(transformer, cols=all(), , exclude_cols=None, allow_reject=False, keep_original=False, rename_columns='{}', n_jobs=None)

Apply a transformer to selected columns in a dataframe.

This transformer applies the given transformer to all the selected columns in
the input dataframe; non-selected columns are passed through without modification.
By default, all selected columns are passed to the same transformer; if the
transformer is a [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer), a
separate clone of the transformer is created for each selected column and
fitted to that column independently.

Refer to the documentation of [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer) for more
details on single-column transformers and how to create them.

* **Parameters:**
  **transformer**
  : The transformer to apply to the selected columns.

  **cols**
  : The columns to attempt to transform. The transformer is applied to the
    columns matched by `cols` and not matched by `exclude_cols`.
    Columns outside this selection are passed through unchanged
    (`fit_transform` is not called on them) and remain unmodified in the
    output. The default is to attempt transforming all columns.

  **exclude_cols**
  : Columns to exclude from transformation. The transformed columns are the
    ones matched by `cols` and not matched by `exclude_cols`.

  **allow_reject**
  : Whether to allow refusing to transform columns for which the provided
    transformer is not suited, for example rejecting non-datetime columns if
    transformer is a DatetimeEncoder. Only relevant if the transformer is a
    [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer). Rejected columns are passed through
    unchanged.

  **keep_original**
  : If `True`, the original columns are preserved in the output. If the
    transformer produces a column with the same name, the transformation
    result is renamed so that both columns can appear in the output. If
    `False`, when the transformer accepts a column, only the
    transformer’s output is included in the result, not the original
    column. In all cases rejected columns (or columns not selected by
    `cols`) are passed through.

  **rename_columns**
  : Format string applied to all transformation output column names. For
    example pass `'transformed_{}'` to prepend `'transformed_'` to all
    output column names. The default value does not modify the names.
    Renaming is not applied to columns not selected by `cols`.

  **n_jobs**
  : Number of jobs to run in parallel.
    `None` means 1 unless in a joblib `parallel_backend` context.
    `-1` means using all processors.
    Note that this parameter is only used when the transformer
    is a [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer).
* **Attributes:**
  **all_inputs_**
  : All column names in the input dataframe.

  **used_inputs_**
  : The names of columns that were transformed.

  **all_outputs_**
  : All column names in the output dataframe.

  **created_outputs_**
  : The names of columns in the output dataframe that were created by one
    of the fitted transformers.

  **transformers_**
  : Maps the name of each column that was transformed to the corresponding
    fitted transformer. Only available when the transformer is a
    [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer).

  **input_to_outputs_**
  : Maps the name of each column that was transformed to the list of the
    resulting columns’ names in the output. Only available when the
    transformer is a [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer).

  **output_to_input_**
  : Maps the name of each column in the transformed output to the name of
    the input column from which it was derived. Only available when the
    transformer is a [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer).

  **transformer_**
  : The fitted transformer. Only available when the transformer is **not** a
    [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer).

#### SEE ALSO
[`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer)
: Base class for single-column transformers, which allows to define custom logic to be applied to each column independently, and to indicate that a column cannot be transformed by raising [`RejectColumn`](skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) exceptions.

### Notes

All columns not selected by `cols` or matched by `exclude_cols` remain
unmodified in the output. Moreover, if `allow_reject` is `True` and
the transformers’
`fit_transform` raises a [`RejectColumn`](skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) exception for a particular
column, that column is passed through unchanged. If `allow_reject` is
`False`, [`RejectColumn`](skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) exceptions are propagated, like other errors
raised by the transformer.

### Examples

Consider the following dataframe:

```pycon
>>> import pandas as pd
>>> from skrub import ApplyToCols
```

```pycon
>>> df = pd.DataFrame(dict(
...     A=[-10., 10.], B=[-10., 0.], C=[19, 20], city=["Paris", "Rome"],
...     D=pd.to_datetime(["2024-05-13T12:05:36", "2024-05-15T13:46:02"]))
... )
>>> df
    A     B   C   city                   D
0 -10.0 -10.0  19  Paris 2024-05-13 12:05:36
1  10.0   0.0  20   Rome 2024-05-15 13:46:02
```

We can apply a [`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder) to the string column “city” by selecting
it with the `cols` parameter:

```pycon
>>> from skrub import StringEncoder
>>> string_encoder = ApplyToCols(StringEncoder(n_components=2), cols=["city"])
>>> df_enc = string_encoder.fit_transform(df)
>>> df_enc
    A     B   C    city_0    city_1                   D
0 -10.0 -10.0  19  1.414214  1.414214 2024-05-13 12:05:36
1  10.0   0.0  20  0.000000  0.000000 2024-05-15 13:46:02
```

Since we selected only column “city”, the transformer was applied only to that
column, while the other columns were left unchanged.

Scikit-learn transformers that can be applied to multiple columns at once can also
be used with `ApplyToCols`. For example, to apply a
[`sklearn.decomposition.PCA`](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA) to the numeric columns “A”, “B”, and “C”,
we can do:

```pycon
>>> from sklearn.decomposition import PCA
>>> pca = ApplyToCols(PCA(n_components=2), cols=["A", "B", "C"])
>>> pca.fit_transform(df)
    city                   D       pca0          pca1
0  Paris 2024-05-13 12:05:36 -11.191515  1.976705e-16
1   Rome 2024-05-15 13:46:02  11.191515  1.976705e-16
```

Note that the columns “city” and “D” were not modified since they were not
selected.

We can also rely on the skrub selectors to select the columns. For example,
we can use [`numeric()`](skrub.selectors.numerichtml.md#skrub.selectors.numeric) to select all numeric columns:

```pycon
>>> from skrub import selectors as s
>>> from sklearn.preprocessing import StandardScaler
>>> scaler = ApplyToCols(StandardScaler(), cols=s.numeric())
>>> scaler.fit_transform(df)
    city                   D    A    B    C
0  Paris 2024-05-13 12:05:36 -1.0 -1.0 -1.0
1   Rome 2024-05-15 13:46:02  1.0  1.0  1.0
```

We can also exclude columns from a broader selection. For example, if we want
to scale numeric columns, but exclude an integer ID, we can do:

```pycon
>>> df_id = pd.DataFrame(dict(id=[1000, 2000], A=[-10., 10.], B=[-10., 0.], C=[19, 20]))
>>> exc_scaler = ApplyToCols(StandardScaler(), exclude_cols="id")
>>> exc_scaler.fit_transform(df_id)
    id    A    B    C
0  1000 -1.0 -1.0 -1.0
1  2000  1.0  1.0  1.0
```

It is possible to set `allow_reject=True` to allow the transformer to reject
columns it cannot handle. For example, the [`DatetimeEncoder`](skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) cannot handle
columns that do not have datetime as their dtype. We can still apply it to
all the columns by setting `allow_reject=True`; in this case, the rejected
columns are passed through unchanged:

```pycon
>>> from skrub import DatetimeEncoder
>>> datetime = ApplyToCols(DatetimeEncoder(), allow_reject=True)
>>> datetime.fit_transform(df)
    A     B   C   city  D_year  D_month  D_day  D_hour  D_total_seconds
0 -10.0 -10.0  19  Paris  2024.0      5.0   13.0    12.0     1.715602e+09
1  10.0   0.0  20   Rome  2024.0      5.0   15.0    13.0     1.715781e+09
```

If `allow_reject=False` (the default), the same transformation would raise
an error since the transformer cannot handle the columns “A”, “B”, and “C”:

```pycon
>>> datetime = ApplyToCols(DatetimeEncoder(), allow_reject=False)
>>> datetime.fit_transform(df)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'A' does not have Date or Datetime dtype.
Transformer DatetimeEncoder.fit_transform failed on column 'A'. See above for the full traceback.
```

It is often useful to wrap a [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) or [`Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner) in
`ApplyToCols` to select or exclude columns based on patterns. For example,
to apply a [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) to all columns except those ending with “_id”,
we can do:

```pycon
>>> import skrub.selectors as s
>>> from skrub import ApplyToCols, TableVectorizer
```

```pycon
>>> df = pd.DataFrame(dict(
...     user_id=["A001", "A002"],
...     age=[25, 30],
...     department=["Engineering", "Sales"],
... ))
>>> tv = ApplyToCols(TableVectorizer(), exclude_cols=s.glob("*_id"))
>>> tv.fit_transform(df)
    user_id   age   department_Sales
0    A001  25.0               0.0
1    A002  30.0               1.0
```

**Accessing fitted transformers**

Depending on the transformer, the fitted transformers
are stored in different attributes. For single-column transformers, the fitted
transformers are stored in the
`transformers_` attribute as a dictionary mapping column names to fitted
transformers. Columns that were not selected or were rejected do not have a
transformer:

```pycon
>>> string_encoder.transformers_
{'city': StringEncoder(n_components=2)}
```

In all other cases, the fitted transformer is stored in the `transformer_`
attribute:

```pycon
>>> scaler.transformer_
StandardScaler()
```

If a single-column transformer can be applied to multiple columns, for example
if there are multiple string columns, the transformer provided to `ApplyToCols`
is cloned and fitted separately to each column.

```pycon
>>> df_str = pd.DataFrame(dict(C1=["a", "b"], C2=["c", "d"]))
>>> se = ApplyToCols(StringEncoder(n_components=2))
>>> se.fit_transform(df_str)
    C1_0      C1_1      C2_0      C2_1
0  1.414214  0.000000  1.414214  0.000000
1  0.000000  1.414214  0.000000  1.414214
```

Then, each fitted transformer is stored in the `transformers_` attribute:

```pycon
>>> se.transformers_
{'C1': StringEncoder(n_components=2), 'C2': StringEncoder(n_components=2)}
```

**Renaming outputs & keeping the original columns**

The `rename_columns` parameter allows renaming output columns.

```pycon
>>> df = pd.DataFrame(dict(A=[-10., 10.], B=[0., 100.]))
>>> scaler = ApplyToCols(StandardScaler(), rename_columns='{}_scaled')
>>> scaler.fit_transform(df)
   A_scaled  B_scaled
0      -1.0      -1.0
1       1.0       1.0
```

The renaming is only applied to columns selected by `cols` (and not
rejected by the transformer when `allow_reject` is `True`).

```pycon
>>> scaler = ApplyToCols(StandardScaler(), cols=['A'], rename_columns='{}_scaled')
>>> scaler.fit_transform(df)
    B  A_scaled
0    0.0      -1.0
1  100.0       1.0
```

`rename_columns` can be particularly useful when `keep_original` is
`True`. When a column is transformed, we can tell `ApplyToCols` to
retain the original, untransformed column in the output. If the transformer
produces a column with the same name, the transformation result is renamed
to avoid a name clash.

```pycon
>>> scaler = ApplyToCols(StandardScaler(), keep_original=True)
>>> scaler.fit_transform(df)
      A  A__skrub_89725c56__      B  B__skrub_81cc7d00__
0 -10.0                 -1.0    0.0                 -1.0
1  10.0                  1.0  100.0                  1.0
```

In this case we may want to set a more sensible name for the transformer’s output:

```pycon
>>> scaler = ApplyToCols(
...     StandardScaler(), keep_original=True, rename_columns="{}_scaled"
... )
>>> scaler.fit_transform(df)
      A      B  A_scaled  B_scaled
0 -10.0    0.0      -1.0      -1.0
1  10.0  100.0       1.0       1.0
```

### Methods

| [`fit`](#skrub.ApplyToCols.fit)(X[, y])                                               | Fit the transformer to the data.                    |
|---------------------------------------------------------------------------------------|-----------------------------------------------------|
| [`fit_transform`](#skrub.ApplyToCols.fit_transform)(X[, y])                           | Fit the transformer on all columns and transform X. |
| [`get_feature_names_out`](#skrub.ApplyToCols.get_feature_names_out)([input_features]) | Get output feature names for transformation.        |
| [`get_params`](#skrub.ApplyToCols.get_params)([deep])                                 | Get parameters for this estimator.                  |
| [`set_output`](#skrub.ApplyToCols.set_output)(\*[, transform])                        | Set output container.                               |
| [`set_params`](#skrub.ApplyToCols.set_params)(\*\*params)                             | Set the parameters of this estimator.               |
| [`transform`](#skrub.ApplyToCols.transform)(X)                                        | Transform a dataframe.                              |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None, \*\*kwargs)

Fit the transformer to the data.

* **Parameters:**
  **X**
  : The data to transform.

  **y**
  : The target data.

  **\*\*kwargs**
  : Extra named arguments are passed to the `fit_transform()` method of
    the individual column transformers (the clones of `self.transformer`).
* **Returns:**
  ApplyToCols
  : The transformer itself.

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None, \*\*kwargs)

Fit the transformer on all columns and transform X.

* **Parameters:**
  **X**
  : The data to transform.

  **y**
  : The target data.

  **\*\*kwargs**
  : Extra named arguments are passed to the `fit_transform()` method
    of `self.transformer`.
* **Returns:**
  **result**
  : The transformed data.

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

Transform a dataframe.

* **Parameters:**
  **X**
  : The column to transform.

  **\*\*kwargs**
  : Extra named arguments are passed to the `transform()`.
* **Returns:**
  **result**
  : The transformed data.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
