# skrub.DataOp.skb.make_grid_search

#### DataOp.skb.make_grid_search(, fitted=False, keep_subsampling=False, \*\*kwargs)

Find the best parameters with grid search.

This function returns a [`ParamSearch`](skrub.ParamSearchhtml.md#skrub.ParamSearch), an object similar to
scikit-learn’s [`GridSearchCV`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html#sklearn.model_selection.GridSearchCV), where
the main difference is that `fit()` and `predict()` accept a
dictionary of inputs rather than `X` and `y`. The best learner can
be returned by calling `.best_learner_`.

* **Parameters:**
  **fitted**
  : If `True`, the gridsearch is fitted on the data provided when
    initializing variables in this DataOp (the data returned by
    `.skb.get_data()`).

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), fit on a subsample of the data. By
    default subsampling is not applied and all the data is used. This
    is only applied for fitting the grid search when `fitted=True`,
    subsequent use of the grid search is not affected by subsampling.
    Therefore it is an error to pass `keep_subsampling=True` and
    `fitted=False` (because `keep_subsampling=True` would have no
    effect).

  **kwargs**
  : All other named arguments are forwarded to
    `sklearn.search.GridSearchCV`.
* **Returns:**
  ParamSearch
  : An object implementing the hyperparameter search. Besides the usual
    `fit`, `predict`, attributes of interest are

  `results_`, `plot_results()`, and `best_learner_`.

#### SEE ALSO
[`skrub.DataOp.skb.make_randomized_search`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Find the best parameters with randomized search.

### Examples

```pycon
>>> import skrub
>>> from sklearn.datasets import make_classification
>>> from sklearn.linear_model import LogisticRegression
>>> from sklearn.ensemble import RandomForestClassifier
>>> from sklearn.dummy import DummyClassifier
```

```pycon
>>> X_a, y_a = make_classification(random_state=0)
>>> X, y = skrub.X(X_a), skrub.y(y_a)
>>> logistic = LogisticRegression(C=skrub.choose_from([0.1, 10.0], name="C"))
>>> rf = RandomForestClassifier(
...     n_estimators=skrub.choose_from([3, 30], name="N 🌴"),
...     random_state=0,
... )
>>> classifier = skrub.choose_from(
...     {"logistic": logistic, "rf": rf, "dummy": DummyClassifier()}, name="classifier"
... )
>>> pred = X.skb.apply(classifier, y=y)
>>> print(pred.skb.describe_param_grid())
- classifier: 'logistic'
  C: [0.1, 10.0]
- classifier: 'rf'
  N 🌴: [3, 30]
- classifier: 'dummy'
```

```pycon
>>> search = pred.skb.make_grid_search(fitted=True)
>>> search.results_
      C   N 🌴 classifier mean_test_score
0   NaN  30.0         rf             0.89
1   0.1   NaN   logistic             0.84
2  10.0   NaN   logistic             0.80
3   NaN   3.0         rf             0.65
4   NaN   NaN      dummy             0.50
```

If the DataOp contains some numeric ranges (`choose_float`,
`choose_int`), either discretize them by providing the `n_steps`
argument or use `make_randomized_search` instead of
`make_grid_search`.

```pycon
>>> logistic = LogisticRegression(
...     C=skrub.choose_float(0.1, 10.0, log=True, n_steps=5, name="C")
... )
>>> pred = X.skb.apply(logistic, y=y)
>>> print(pred.skb.describe_param_grid())
- C: choose_float(0.1, 10.0, log=True, n_steps=5, name='C')
>>> search = pred.skb.make_grid_search(fitted=True)
>>> search.results_
    C   mean_test_score
0       0.100000        0.84
1       0.316228        0.83
2       1.000000        0.81
3       3.162278        0.80
4       10.000000       0.80
```

Please refer to the examples gallery for an in-depth explanation.

<!-- !! processed by numpydoc !! -->
