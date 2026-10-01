# skrub.DataOp.skb.describe_defaults

#### DataOp.skb.describe_defaults()

Describe the hyper-parameters used by the default learner.

Returns a dict mapping choice names to a simplified representation of
the corresponding value in the default learner.

### Examples

```pycon
>>> import skrub
>>> from sklearn.datasets import make_classification
>>> from sklearn.linear_model import LogisticRegression
>>> from sklearn.feature_selection import SelectKBest
>>> from sklearn.ensemble import RandomForestClassifier
```

```pycon
>>> X, y = skrub.X(), skrub.y()
>>> selector = SelectKBest(k=skrub.choose_int(4, 20, log=True, name='k'))
>>> logistic = LogisticRegression(
...     C=skrub.choose_float(0.1, 10.0, log=True, name="C"),
... )
>>> rf = RandomForestClassifier(
...     n_estimators=skrub.choose_int(3, 30, log=True, name="N 🌴"),
...     random_state=0,
... )
>>> classifier = skrub.choose_from(
...     {"logistic": logistic, "rf": rf}, name="classifier"
... )
>>> pred = X.skb.apply(selector, y=y).skb.apply(classifier, y=y)
>>> print(pred.skb.describe_defaults())
{'k': 9, 'C': 1.0..., 'classifier': 'logistic'}
```

<!-- !! processed by numpydoc !! -->
