# skrub.DataOp.skb.with_scoring

#### DataOp.skb.with_scoring(scoring, kwargs=None, name=None)

Attach a scoring method to this DataOp.

This records a scikit-learn
[scorer](https://scikit-learn.org/stable/modules/model_evaluation.html#the-scoring-parameter-defining-model-evaluation-rules)
to be used when measuring the quality of predictions.

It allows configuring the default scoring metrics, and most importantly
to compute any additional arguments needed for scoring (such as sample
weights) as part of the DataOp evaluation.

Calls to this method can be chained to add multiple scorers that
use different kwargs. Several scoring strategies can be passed to a
single calls if they do not need different kwargs (see the description
of the `scoring` parameter).

* **Parameters:**
  **scoring**
  : Strategy to evaluate the performance of the learner. This accepts
    exactly the same inputs as
    [`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate):
    <br/>
    If `scoring` represents a single score, one can use:
    - a single string;
    - a callable that returns a single value.
    - `None`, the estimator’s default evaluation criterion is used.
    <br/>
    If `scoring` represents multiple scores, one can use:
    - a list or tuple of unique strings;
    - a callable returning a dictionary where the keys are the metric
      names and the values are the metric scores;
    - a dictionary with metric names as keys and callables a values.
    <br/>
    See the scikit-learn
    [documentation](https://scikit-learn.org/stable/modules/model_evaluation.html#scoring-api-overview)
    on scoring for details.
    <br/>
    `scoring` can be a DataOp, so it is possible to pass something like
    `skrub.deferred(sklearn.metrics.make_scorer)(...)`, although it
    is usually simpler to pass an actual value for `scoring` and rely
    on `kwargs` for any inputs that need to be computed dynamically.

  **kwargs**
  : Additional named arguments to be passed to the scorer. A typical
    example is `sample_weight`, used by many scikit-learn metrics.
    Note that `kwargs` can be a DataOp that will be evaluated
    when scoring the learner, so scorer inputs such as sample weights
    can be dynamic. None is the same as an empty dict

  **name**
  : A name used when displaying scoring results. If `scoring`
    represents multiple metrics, `name` will be prepended to each of
    the metrics’ name.
* **Returns:**
  DataOp
  : A DataOp that performs the same transformations and prediction, but
    with scoring information attached to it, which will be used for
    example in cross-validation.

### Notes

If this method is used several times, all calls to it must be grouped
– there can be no other nodes in-between. For example
`pred.skb.score_with('accuracy').skb.score_with('roc_auc')` is allowed,
whereas
`pred.skb.score_with('accuracy').skb.apply_func(a_function).skb.score_with('roc_auc')` is not.
Typically all the `score_with` calls happen at the very end of the
DataOp construction.

### Examples

```pycon
>>> import skrub
>>> from sklearn.dummy import DummyClassifier
```

```pycon
>>> df = skrub.datasets.toy_products()
>>> data = skrub.var("df", df)
>>> X = data[["description", "price"]].skb.mark_as_X(cv=2)
>>> y = data["category"].skb.mark_as_y()
>>> pred = X.skb.apply(DummyClassifier(), y=y)
```

We can get the accuracy (the default metric) with 2-fold cross-validation:

```pycon
>>> pred.skb.cross_validate()
   fit_time  score_time  test_score
0  0.003982    0.002405    0.666667
1  0.002582    0.002169    0.666667
```

But suppose we want to give more importance to the pricier products. We can
weight the accuracy by the item price:

```pycon
>>> sample_weight = X["price"]
>>> pred.skb.with_scoring(
...     "accuracy", kwargs={"sample_weight": sample_weight}
... ).skb.cross_validate()
   fit_time  score_time  test_accuracy
0  0.003045    0.003275       0.888889
1  0.002659    0.003026       0.647059
```

We can also have both by calling `with_scoring` twice:

```pycon
>>> pred.skb.with_scoring("accuracy").skb.with_scoring(
...     "accuracy",
...     kwargs={"sample_weight": sample_weight},
...     name="weighted_accuracy",
... ).skb.cross_validate()
   fit_time  score_time  test_accuracy  test_weighted_accuracy
0  0.002738    0.005733       0.666667                0.888889
1  0.002845    0.005705       0.666667                0.647059
```

When we have several scorers that use the same kwargs, we can pass a list
or dict of metrics, as for [`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate):

```pycon
>>> sample_weight = X["price"]
>>> pred.skb.with_scoring(
...     ["accuracy", "neg_log_loss"], kwargs={"sample_weight": sample_weight}
... ).skb.cross_validate()
   fit_time  score_time  test_accuracy  test_neg_log_loss
0  0.002694    0.007017       0.888889          -0.482481
1  0.002833    0.006627       0.647059          -0.650105
```

<!-- !! processed by numpydoc !! -->
