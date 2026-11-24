# skrub.DataOp.skb.subsample

#### DataOp.skb.subsample(n=1000, , how='head')

Configure subsampling of a dataframe or numpy array.

Enables faster development by computing the previews on a subsample of
the available data. Outside of previews, no subsampling takes place by
default but it can be turned on with the `keep_subsampling` parameter
– see the Notes section for details.

* **Parameters:**
  **n**
  : Number of rows to keep.

  **how**
  : How subsampling should be done (when it takes place). If ‘head’,
    the first `n` rows are kept. If ‘random’, `n` rows are sampled
    randomly, without maintaining order and without replacement.
* **Returns:**
  subsampled data
  : The subsampled dataframe, column or numpy array.

#### SEE ALSO
[`DataOp.skb.preview`](skrub.DataOp.skb.previewhtml.md#skrub.DataOp.skb.preview)
: Access a preview of the result on the subsampled data.

### Notes

This method configures *how* the dataframe should be subsampled. If it
has been configured, subsampling actually only takes place in some
specific situations:

- When computing the previews (results displayed when printing a
  DataOp and the output of [`DataOp.skb.preview()`](skrub.DataOp.skb.previewhtml.md#skrub.DataOp.skb.preview)).
- When it is explicitly requested by passing `keep_subsampling=True` to one
  of the functions that expose that parameter such as
  [`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search) or [`cross_validate()`](skrub.cross_validatehtml.md#skrub.cross_validate).

When subsampling has not been configured (`subsample` has not
been called anywhere in the DataOp plan), no subsampling is ever done.

Subsampling is never performed during inference (using the `predict` or `score` methods), as this would
lead to inconsistent shapes (number of samples) between the predictions
and the ground truth labels.

This method can only be used on steps that produce a dataframe, a
column (series) or a numpy array.

Note that subsampling is local to the variable that is being subsampled.
This means that, if two variables are meant to have the same number of
rows (e.g., `X` and `y`), both should be subsampled with the same
strategy.

The seed for the `random` strategy is fixed, so the sampled rows are
constant when sampling across different variables.

### Examples

```pycon
>>> from sklearn.datasets import load_diabetes
>>> from sklearn.linear_model import Ridge
>>> import skrub
```

```pycon
>>> df = load_diabetes(as_frame=True)["frame"]
>>> df.shape
(442, 11)
```

```pycon
>>> data = skrub.var("data", df).skb.subsample(n=15)
```

We can see that the previews use only a subsample of 15 rows:

```pycon
>>> data.shape
<GetAttr 'shape'>
Result (on a subsample):
――――――――――――――――――――――――
(15, 11)
>>> X = data.drop("target", axis=1, errors="ignore").skb.mark_as_X()
>>> y = data["target"].skb.mark_as_y()
>>> pred = X.skb.apply(
...     Ridge(alpha=skrub.choose_float(0.01, 10.0, log=True, name="α")), y=y
... )
```

Here also, the preview for the predictions contains 15 rows:

```pycon
>>> pred
<Apply Ridge>
Result (on a subsample):
――――――――――――――――――――――――
0     142.866906
1     130.980765
2     138.555388
3     149.703363
4     136.015214
5     139.773213
6     134.110415
7     129.224783
8     140.161363
9     155.272033
10    139.552110
11    130.318783
12    135.956591
13    142.998060
14    132.511013
Name: target, dtype: float64
```

By default, model fitting and hyperparameter search are done on the
full data, so if we want the subsampling to take place we have to
pass `keep_subsampling=True`:

```pycon
>>> quick_search = pred.skb.make_randomized_search(
...     keep_subsampling=True, fitted=True, n_iter=4, random_state=0
... )
>>> quick_search.detailed_results_[["mean_test_score", "mean_fit_time", "α"]]
   mean_test_score  mean_fit_time         α
0        -0.597596       0.004322  0.431171
1        -0.599036       0.004328  0.443038
2        -0.615900       0.004272  0.643117
3        -0.637498       0.004219  1.398196
```

Now that we have checked our learner works on a subsample, we can
fit the hyperparameter search on the full data:

```pycon
>>> full_search = pred.skb.make_randomized_search(
...     fitted=True, n_iter=4, random_state=0
... )
>>> full_search.detailed_results_[["mean_test_score", "mean_fit_time", "α"]]
   mean_test_score  mean_fit_time         α
0         0.457807       0.004791  0.431171
1         0.456808       0.004834  0.443038
2         0.439670       0.004849  0.643117
3         0.380719       0.004827  1.398196
```

This example dataset is so small that the subsampling does not change
the fit computation time but we can tell the second search used the
full data from the higher scores. For datasets of a realistic size
using the subsampling allows us to do a “dry run” of the
cross-validation or model fitting much faster than when using the
full data.

Sampling only one variable does not sample the other:

```pycon
>>> data = skrub.var("data", df)
>>> X = data.drop("target", axis=1, errors="ignore").skb.mark_as_X()
>>> X = X.skb.subsample(n=15)
>>> y = data["target"].skb.mark_as_y()
>>> X.shape
<GetAttr 'shape'>
Result (on a subsample):
――――――――――――――――――――――――
(15, 10)
>>> y.shape
<GetAttr 'shape'>
Result:
―――――――
(442,)
```

Read more about subsampling in the [User Guide](../../modules/data_ops/ml_pipeline/subsampling_datahtml.md#user-guide-data-ops-subsampling).

<!-- !! processed by numpydoc !! -->
