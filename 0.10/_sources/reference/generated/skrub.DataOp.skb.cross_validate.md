# skrub.DataOp.skb.cross_validate

#### DataOp.skb.cross_validate(environment=None, , keep_subsampling=False, \*\*kwargs)

Cross-validate the DataOp plan.

This generates the learner with default hyperparameters and runs
scikit-learn cross-validation.

* **Parameters:**
  **environment**
  : Bindings for variables contained in the DataOp plan. If not
    provided, the values passed when initializing `var()` are
    used.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), use a subsample of the data. By
    default subsampling is not applied and all the data is used.

  **kwargs**
  : All other named arguments are forwarded to
    `sklearn.model_selection.cross_validate`, except that
    scikit-learn’s `return_estimator` parameter is named
    `return_learner` here.
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : Cross-validation results.

#### SEE ALSO
[`DataOp.skb.train_test_split()`](skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split)
: Prepare training and testing sets for a DataOp.

[`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Perform hyperparameter tuning driven by cross-validation scores.

[`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Perform hyperparameter tuning driven by cross-validation scores.

### Examples

```pycon
>>> from sklearn.datasets import make_classification
>>> from sklearn.linear_model import LogisticRegression
>>> import skrub
```

```pycon
>>> X_a, y_a = make_classification(random_state=0)
>>> X, y = skrub.X(X_a), skrub.y(y_a)
>>> pred = X.skb.apply(LogisticRegression(), y=y)
>>> pred.skb.cross_validate(cv=2)['test_score']
0    0.84
1    0.78
Name: test_score, dtype: float64
```

Passing some data:

```pycon
>>> data = {'X': X_a, 'y': y_a}
>>> pred.skb.cross_validate(data)['test_score']
0    0.75
1    0.90
2    0.85
3    0.65
4    0.90
Name: test_score, dtype: float64
```

<!-- !! processed by numpydoc !! -->
