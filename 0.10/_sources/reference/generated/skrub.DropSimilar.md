# DropSimilar

### *class* skrub.DropSimilar(threshold=1)

Drop columns found too redundant to the rest of the dataframe,
according to association defined by Cramér’s V.

This is done by computing Cramér’s V between every possible two columns,
and sorting these couples in descending order. Then, for every association above
the given threshold, one of the two columns is dropped.

* **Parameters:**
  **threshold**
  : The Cramér association score value above which to start dropping columns.
* **Attributes:**
  **to_drop_**
  : The names of columns evaluated for removal

  **all_outputs_**
  : The names of columns that the transformer keeps

  **column_associations_**
  : A dataframe with columns 
    <br/>
    ```
    `
    ```
    <br/>
    left_column_name’, ‘right_column_name’ and ‘cramer_v’
    listing association scores between every pair of columns.

#### SEE ALSO
[`DropUninformative`](skrub.DropUninformativehtml.md#skrub.DropUninformative)
: Drops a single column if certain criteria indicate that it contains little to no information (amount of nulls, of distinct values…)

[`Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: Runs several checks to sanitize a dataframe, including converting columns to standard formats or dropping certain columns.

### Examples

```pycon
>>> from skrub import DropSimilar
>>> from skrub.datasets import toy_cities
>>> df = toy_cities(size=5000, n_metrics=0)
>>> df.head()
          uid   cities  encoded_cities               start                 end
0  SHAoqcdajQ  Vilnius            17.0 2004-09-02 03:22:56 2014-06-26 11:36:43
1  HVAFYLGCDW      NaN             NaN 1979-10-22 01:43:56 1987-10-15 06:30:05
2  oQIauSCbNL     Rome            13.0 1986-08-09 19:01:10 2002-04-06 03:56:09
3  SjeSbCepzv  Vilnius            17.0 2008-11-26 15:57:13 2021-11-16 23:16:13
4  ubagaIBHnG   London             8.0 1982-09-13 20:54:54 2000-12-02 07:36:41
```

```pycon
>>> ds = DropSimilar(threshold=0.8)
>>> clean_df = ds.fit_transform(df)
```

`ds` has now removed a column for each pair with association above 0.8.
These associations are stored in the `table_associations_` attribute:

```pycon
>>> ds.table_associations_.head()
  left_column_name right_column_name  cramer_v
0           cities    encoded_cities  1.000000
1              uid            cities  0.052979
2              uid    encoded_cities  0.052979
3   encoded_cities               end  0.046455
4           cities               end  0.046455
```

A single pair is above the threshold, `cities` and `encoded_cities`,
with an association score of 1. Since one is an encoding of the other,
this is to be expected.
Therefore, one of these two has been marked as dropped by `ds`:

```pycon
>>> ds.to_drop_
['encoded_cities']
```

This leaves us with the shortened dataframe:

```pycon
>>> clean_df.head()
          uid   cities               start                 end
0  SHAoqcdajQ  Vilnius 2004-09-02 03:22:56 2014-06-26 11:36:43
1  HVAFYLGCDW      NaN 1979-10-22 01:43:56 1987-10-15 06:30:05
2  oQIauSCbNL     Rome 1986-08-09 19:01:10 2002-04-06 03:56:09
3  SjeSbCepzv  Vilnius 2008-11-26 15:57:13 2021-11-16 23:16:13
4  ubagaIBHnG   London 1982-09-13 20:54:54 2000-12-02 07:36:41
```

### Methods

| [`fit_transform`](#skrub.DropSimilar.fit_transform)(X[, y])    | Fit to data, then transform it.       |
|----------------------------------------------------------------|---------------------------------------|
| [`get_params`](#skrub.DropSimilar.get_params)([deep])          | Get parameters for this estimator.    |
| [`set_output`](#skrub.DropSimilar.set_output)(\*[, transform]) | Set output container.                 |
| [`set_params`](#skrub.DropSimilar.set_params)(\*\*params)      | Set the parameters of this estimator. |

| **fit**                   |    |
|---------------------------|----|
| **get_feature_names_out** |    |
| **transform**             |    |
<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

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
