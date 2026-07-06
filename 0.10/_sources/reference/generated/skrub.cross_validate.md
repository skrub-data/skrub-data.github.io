# cross_validate

### skrub.cross_validate(learner, environment, , keep_subsampling=False, cv=None, \*\*kwargs)

Cross-validate a learner built from a DataOp.

This runs cross-validation from a learner that was built from a skrub
DataOp with [`make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner), [`make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
or [`make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search).

It is useful to run nested cross-validation of a grid search or randomized
search.

* **Parameters:**
  **learner**
  : A learner generated from a skrub DataOp.

  **environment**
  : Bindings for variables contained in the DataOp.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), use a subsample of the data. By
    default subsampling is not applied and all the data is used.

  **cv**
  : Cross-validation splitting strategy. It can be:
    - None: 5-fold (stratified) cross-validation
    - integer: specify the number of folds
    - sklearn [CV splitter](https://scikit-learn.org/stable/modules/cross_validation.html#cross-validation-iterators)
    - iterable yielding (train, test) splits as arrays of indices.

  **kwargs**
  : All other named arguments are forwarded to
    [`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate), except that scikit-learn’s
    `return_estimator` parameter is named `return_learner` here.
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : Cross-validation results.

#### SEE ALSO
[`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate)
: Evaluate metric(s) by cross-validation and also record fit/score times.

[`make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)
: Get a skrub learner for this DataOp.

[`make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Find the best parameters with grid search.

[`make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Find the best parameters with grid search.

### Examples

```pycon
>>> from sklearn.datasets import make_classification
>>> from sklearn.linear_model import LogisticRegression
>>> import skrub
```

```pycon
>>> X_a, y_a = make_classification(random_state=0)
>>> X, y = skrub.X(X_a), skrub.y(y_a)
>>> log_reg = LogisticRegression(
...     **skrub.choose_float(0.01, 1.0, log=True, name="C")
... )
>>> pred = X.skb.apply(log_reg, y=y)
>>> search = pred.skb.make_randomized_search(random_state=0)
>>> skrub.cross_validate(search, pred.skb.get_data())['test_score']
0    0.75
1    0.90
2    0.95
3    0.75
4    0.85
Name: test_score, dtype: float64
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div>
<!-- thumbnail-parent-div-close --></div>
