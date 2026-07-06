# skrub.DataOp.skb.apply

#### DataOp.skb.apply(estimator, , y=None, cols=all(), exclude_cols=None, no_wrap=False, how='auto', allow_reject=False, unsupervised=False, fit_kwargs=None, fit_transform_kwargs=None, transform_kwargs=None, predict_kwargs=None, predict_proba_kwargs=None, decision_function_kwargs=None, score_kwargs=None)

Apply an estimator that follows the scikit-learn API to a dataframe or numpy array.

* **Parameters:**
  **estimator**
  : The transformer or predictor to apply.

  **y**
  : The prediction targets when `estimator` is a supervised estimator.

  **cols**
  : The columns to transform, when `estimator` is a transformer.

  **exclude_cols**
  : When `estimator` is a transformer, columns to which it should
    \_not_ be applied. The columns that are matched by `cols` AND not
    matched by `exclude_cols` are transformed.

  **no_wrap**
  : Disable wrapping of transformers in [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols).
    <br/>
    By default, when `estimator` is a transformer and the input is a
    DataFrame, the transformer is wrapped in an instance of
    [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols), which allows applying it to part of the
    dataframe only through the `cols` and `allow_reject`
    parameters. Passing `no_wrap=True` disables this wrapping in all
    cases. When `no_wrap` is True, `cols` and `allow_reject`
    cannot be used.

  **how**
  : Deprecated. Use `no_wrap` instead.
    <br/>
    How the estimator is applied. In most cases the default “auto”
    is appropriate.
    - “cols” means `estimator` is wrapped in a `ApplyToEachCol`
      transformer, which fits a separate clone of `estimator` each
      column in `cols`. `estimator` must be a transformer (have a
      `fit_transform` method).
    - “frame” means `estimator` is wrapped in a `ApplyToSubFrame`
      transformer, which fits a single clone of `estimator` to the
      selected part of the input dataframe. `estimator` must be a
      transformer.
    - “no_wrap” means no wrapping, `estimator` is applied directly to
      the unmodified input.
    - “auto” chooses the wrapping depending on the input and estimator.
      If the input is not a dataframe or the estimator is not a
      transformer, the “no_wrap” strategy is chosen. Otherwise if the
      estimator has a `__single_column_transformer__` attribute,
      “cols” is chosen. Otherwise “frame” is chosen.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0.

  **allow_reject**
  : Whether the transformer can refuse to transform columns for which
    it does not apply, in which case they are passed through unchanged.
    This can be useful to avoid specifying exactly which columns should
    be transformed. For example if we apply [`ToDatetime()`](skrub.ToDatetimehtml.md#skrub.ToDatetime)
    to all columns with `allow_reject=True`, string columns that can be
    parsed as dates will be converted and all other columns will be
    passed through. If we use `allow_reject=False` (the default), an
    error would be raised if the dataframe contains columns for which
    [`ToDatetime()`](skrub.ToDatetimehtml.md#skrub.ToDatetime) does not apply (eg a column of numbers).

  **unsupervised**
  : Use this to indicate that `y` is required for scoring but not
    fitting, as is the case for clustering algorithms. If `y` is not
    required at all (for example when applying an unsupervised
    transformer, or when we are not interested in scoring with
    ground-truth labels), simply leave the default `y=None` and there
    is no need to pass a value for `unsupervised`.

  **fit_kwargs**
  : Extra named arguments to pass to the estimator’s `fit()` method,
    for example `fit_kwargs={'sample_weights': [.1, .5, .4]}`. May be
    (or contain) a DataOp, which will be evaluated before passing the
    kwargs to `fit`.

  **fit_transform_kwargs**
  : Extra named arguments for `fit_transform`. See the description of
    the `fit_kwargs` parameter.

  **transform_kwargs**
  : Extra named arguments for `transform`. See the description of the
    `fit_kwargs` parameter.

  **predict_kwargs**
  : Extra named arguments for `predict`. See the description of the
    `fit_kwargs` parameter.

  **predict_proba_kwargs**
  : Extra named arguments for `predict_proba`. See the description of
    the `fit_kwargs` parameter.

  **decision_function_kwargs**
  : Extra named arguments for `decision_function`. See the
    description of the `fit_kwargs` parameter.

  **score_kwargs**
  : Extra named arguments for `score`. See the description of the
    `fit_kwargs` parameter.
* **Returns:**
  result
  : The transformed dataframe when `estimator` is a transformer, and
    the fitted `estimator`’s predictions if it is a supervised
    predictor.

#### SEE ALSO
[`skrub.DataOp.skb.make_learner`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)
: Get a skrub learner for this DataOp.

[`skrub.ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
: Transformer that applies a given estimator to selected columns of a dataframe.

### Examples

```pycon
>>> import skrub
>>> data = skrub.datasets.toy_orders()
>>> x = skrub.X(data.X)
>>> x
<Var 'X'>
Result:
―――――――
   ID product  quantity        date
0   1     pen         2  2020-04-03
1   2     cup         3  2020-04-04
2   3     cup         5  2020-04-04
3   4   spoon         1  2020-04-05
```

```pycon
>>> datetime_encoder = skrub.DatetimeEncoder(add_total_seconds=False)
>>> x.skb.apply(skrub.TableVectorizer(datetime=datetime_encoder))
<Apply TableVectorizer>
Result:
―――――――
    ID  product_cup  product_pen  ...  date_year  date_month  date_day
0  1.0          0.0          1.0  ...     2020.0         4.0       3.0
1  2.0          1.0          0.0  ...     2020.0         4.0       4.0
2  3.0          1.0          0.0  ...     2020.0         4.0       4.0
3  4.0          0.0          0.0  ...     2020.0         4.0       5.0
```

Transform only the `'product'` column:

```pycon
>>> x.skb.apply(skrub.StringEncoder(n_components=2), cols='product')
<Apply StringEncoder>
Result:
―――――――
   ID     product_0     product_1  quantity        date
0   1 -2.560113e-16  1.000000e+00         2  2020-04-03
1   2  1.000000e+00  7.447602e-17         3  2020-04-04
2   3  1.000000e+00  7.447602e-17         5  2020-04-04
3   4 -3.955170e-16 -8.326673e-17         1  2020-04-05
```

Transform all but the `'ID'` and `'quantity'` columns:

```pycon
>>> x.skb.apply(
...     skrub.StringEncoder(n_components=2), exclude_cols=["ID", "quantity"]
... )
<Apply StringEncoder>
Result:
―――――――
   ID     product_0     product_1  quantity    date_0    date_1
0   1  9.775252e-08  7.830415e-01         2  0.766318 -0.406667
1   2  9.999999e-01  0.000000e+00         3  0.943929  0.330148
2   3  9.999998e-01 -1.490116e-08         5  0.943929  0.330149
3   4  9.910963e-08 -6.219692e-01         1  0.766318 -0.406668
```

More complex selection of the columns to transform, here all numeric
columns except the `'ID'`:

```pycon
>>> from sklearn.preprocessing import StandardScaler
>>> from skrub import selectors as s
```

```pycon
>>> x.skb.apply(StandardScaler(), cols=s.numeric() - "ID")
<Apply StandardScaler>
Result:
―――――――
   ID product        date  quantity
0   1     pen  2020-04-03 -0.507093
1   2     cup  2020-04-04  0.169031
2   3     cup  2020-04-04  1.521278
3   4   spoon  2020-04-05 -1.183216
```

For supervised estimators, pass the targets as the argument for `y`:

```pycon
>>> from sklearn.dummy import DummyClassifier
>>> y = skrub.y(data.y)
>>> y
<Var 'y'>
Result:
―――――――
0    False
1    False
2     True
3    False
Name: delayed, dtype: bool
```

```pycon
>>> x.skb.apply(skrub.TableVectorizer()).skb.apply(DummyClassifier(), y=y)
<Apply DummyClassifier>
Result:
―――――――
0    False
1    False
2    False
3    False
Name: delayed, dtype: bool
```

We can also pass additional keyword arguments to the estimator’s
methods. For example a StandardScaler can be passed sample weights.
We first apply it without weights for comparison:

```pycon
>>> import pandas as pd
>>> X = skrub.var("X", pd.DataFrame({"count": [10, 1], "value": [2.0, -2.0]}))
>>> count, value = X["count"], X[["value"]]
>>> value.skb.apply(StandardScaler())
<Apply StandardScaler>
Result:
―――――――
   value
0    1.0
1   -1.0
```

Now we weight by `count`. Note that `count` is itself a DataOp – the
kwargs, like X and y, can be computed during the DataOp’s evaluation:

```pycon
>>> value.skb.apply(StandardScaler(), fit_transform_kwargs={"sample_weight": count})
<Apply StandardScaler>
Result:
―――――――
      value
0  0.316...
1 -3.162...
```

Another example would be passing evaluation sets to the `fit` method
of an `xgboost` estimator.

Sometimes we want to pass a value for `y` because it is required for
scoring and cross-validation, but it is not needed for fitting the
estimator. In this case pass `unsupervised=True`.

```pycon
>>> from sklearn.datasets import make_blobs
>>> from sklearn.cluster import KMeans
```

```pycon
>>> X, y = make_blobs(n_samples=10, random_state=0)
>>> e = skrub.X(X).skb.apply(
...     KMeans(n_clusters=2, n_init=1, random_state=0),
...     y=skrub.y(y),
...     unsupervised=True,
... )
>>> e.skb.cross_validate()["test_score"]
0   -19.437348
1   -12.463938
2   -11.804288
3   -37.238832
4    -4.857855
Name: test_score, dtype: float64
>>> learner = e.skb.make_learner().fit({"X": X})
>>> learner.predict({"X": X})
array([0, 0, 0, 0, 0, 0, 1, 0, 0, 0], dtype=int32)
```

<!-- !! processed by numpydoc !! -->
