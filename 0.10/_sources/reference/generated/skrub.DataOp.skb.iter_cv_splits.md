# skrub.DataOp.skb.iter_cv_splits

#### DataOp.skb.iter_cv_splits(environment=None, , keep_subsampling=False, cv=None)

Yield splits of an environment into training and testing environments.

* **Parameters:**
  **environment**
  : The environment (dict mapping variable names to values) containing the
    full data. If `None` (the default), the data is retrieved from the
    DataOp.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), use a subsample of the data. By
    default subsampling is not applied and all the data is used.

  **cv**
  : The default is 5-fold without shuffling. Can be a cross-validation
    splitter, an iterable yielding pairs of (train, test) indices, or an
    int to specify the number of folds for KFold splitting.
* **Yields:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : For each split, a dict is produced, containing the following keys:
    - train: a dictionary containing the training environment
    - test: a dictionary containing the test environment
    - X: the value of the variable marked with
      [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) in `environment`, before splitting.
    - y: the value of the variable marked with
      [`mark_as_y()`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y) in `environment`, before
      splitting, if there is one (may not be the case for unsupervised
      learning).
    - X_train: the value of the variable marked with
      [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) in the train environment
    - X_test: the value of the variable marked with
      [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) in the test environment
    - y_train: the value of the variable marked with
      [`mark_as_y()`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y) in
      the train environment, if there is one (may not be the case for
      unsupervised learning).
    - y_test: the value of the variable marked with
      [`mark_as_y()`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y) in
      the test environment, if there is one (may not be the case for
      unsupervised learning).
    - row_indices_train: the row indices (in X and y) of the training samples.
    - row_indices_test: the row indices (in X and y) of the testing samples.

### Examples

```pycon
>>> import skrub
>>> from sklearn.dummy import DummyClassifier
>>> from sklearn.metrics import accuracy_score
```

```pycon
>>> orders = skrub.var("orders")
>>> X = orders.skb.drop("delayed").skb.mark_as_X()
>>> y = orders["delayed"].skb.mark_as_y()
>>> delayed = X.skb.apply(skrub.TableVectorizer()).skb.apply(
...     DummyClassifier(), y=y
... )
>>> df = skrub.datasets.toy_orders().orders
>>> accuracies = []
>>> for split in delayed.skb.iter_cv_splits({"orders": df}, cv=3):
...     learner = delayed.skb.make_learner().fit(split["train"])
...     prediction = learner.predict(split["test"])
...     accuracies.append(accuracy_score(split["y_test"], prediction))
>>> accuracies
[1.0, 0.0, 1.0]
```

<!-- !! processed by numpydoc !! -->
