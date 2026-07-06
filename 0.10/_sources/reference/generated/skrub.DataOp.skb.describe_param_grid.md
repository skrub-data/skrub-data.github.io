# skrub.DataOp.skb.describe_param_grid

#### DataOp.skb.describe_param_grid()

Describe the hyper-parameters extracted from choices in the DataOp.

DataOps can contain choices, ranges of possible values to be tuned
by hyperparameter search. This function provides a description of the
grid (set of combinations) of hyperparameters extracted from the
DataOp.

Please refer to the examples gallery for a full explanation of choices
and hyper-parameter tuning.

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : A textual description of the different choices contained in this
    DataOp.

### Examples

```pycon
>>> from sklearn.linear_model import LogisticRegression
>>> from sklearn.ensemble import RandomForestClassifier
>>> from sklearn.decomposition import PCA
>>> from sklearn.feature_selection import SelectKBest
```

```pycon
>>> import skrub
```

```pycon
>>> X = skrub.X()
>>> y = skrub.y()
```

```pycon
>>> dim_reduction = skrub.choose_from(
...     {
...         "PCA": PCA(
...             n_components=skrub.choose_int(
...                 5, 100, log=True, name="n_components"
...             )
...         ),
...         "SelectKBest": SelectKBest(
...             k=skrub.choose_int(5, 100, log=True, name="k")
...         ),
...     },
...     name="dim_reduction",
... )
>>> selected = X.skb.apply(dim_reduction)
>>> classifier = skrub.choose_from(
...     {
...         "logreg": LogisticRegression(
...             C=skrub.choose_float(0.001, 100, log=True, name="C")
...         ),
...         "rf": RandomForestClassifier(
...             n_estimators=skrub.choose_int(20, 400, name="N 🌴")
...         ),
...     },
...     name="classifier",
... )
>>> pred = selected.skb.apply(classifier, y=y)
>>> print(pred.skb.describe_param_grid())
- dim_reduction: 'PCA'
  n_components: choose_int(5, 100, log=True, name='n_components')
  classifier: 'logreg'
  C: choose_float(0.001, 100, log=True, name='C')
- dim_reduction: 'PCA'
  n_components: choose_int(5, 100, log=True, name='n_components')
  classifier: 'rf'
  N 🌴: choose_int(20, 400, name='N 🌴')
- dim_reduction: 'SelectKBest'
  k: choose_int(5, 100, log=True, name='k')
  classifier: 'logreg'
  C: choose_float(0.001, 100, log=True, name='C')
- dim_reduction: 'SelectKBest'
  k: choose_int(5, 100, log=True, name='k')
  classifier: 'rf'
  N 🌴: choose_int(20, 400, name='N 🌴')
```

Sampling a configuration for this learner starts by selecting an entry
(marked by `-`) in the list above, then a value for each of the
hyperparameters listed (used) in that entry. For example note that the
configurations that use the random forest do not list the
hyperparameter `C` which is used only by the logistic regression.

<!-- !! processed by numpydoc !! -->
