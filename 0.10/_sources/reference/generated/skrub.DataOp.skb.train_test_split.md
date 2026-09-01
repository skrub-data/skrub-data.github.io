# skrub.DataOp.skb.train_test_split

#### DataOp.skb.train_test_split(environment=None, , keep_subsampling=False, split_func=None, \*\*split_func_kwargs)

Split an environment into training and testing environments.

* **Parameters:**
  **environment**
  : The environment (dict mapping variable names to values) containing the
    full data. If `None` (the default), the data is retrieved from the
    DataOp.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), use a subsample of the data. By
    default subsampling is not applied and all the data is used.

  **split_func**
  : The function used to split X and y once they have been computed. If
    `split_func` is not provided,
    - If a cross-validation splitter has been set with
      [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X), that splitter is used.
      In this case the only accepted kwarg in `split_func_kwargs` is
      `'split_index'`, which indicates which split to use. By default
      `'split_index'` is -1, i.e. the last split is used.
    <br/>
      Note: the default split is the last one because when using a
      time series splitter it typically is the one that uses the most data;
      this is particularly important if the train part will be further
      subdivided e.g. for hyperparameter search. (For other splitters
      split ordering is usually arbitrary.)
    - Otherwise [`sklearn.model_selection.train_test_split()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html#sklearn.model_selection.train_test_split) is
      used (and `split_func_kwargs` are forwarded to it).

  **split_func_kwargs**
  : Additional named arguments to pass to the splitting function. When
    relying on the splitter passed to [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X),
    the only accepted kwarg is `'split_index'` which indicates which
    split to use.
    Kwargs for the `split()` method itself should be passed to
    `mark_as_X`.
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : The return value is slightly different than scikit-learn’s. Rather than
    a tuple, it returns a dictionary with the following keys:
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
    <br/>
    If relying on the splitter passed to [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X),
    the following keys are added:
    - row_indices_train: the row indices (in X and y) of the training samples.
    - row_indices_test: the row indices (in X and y) of the testing samples.
    <br/>
    Those are not available otherwise, as
    [`sklearn.model_selection.train_test_split()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html#sklearn.model_selection.train_test_split) only returns
    X_train, X_test, etc. data, not the indices.

#### SEE ALSO
[`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X)
: Mark a variable as the input features (X) for training and testing. This function can also take a custom splitter.

[`cross_validate()`](skrub.DataOp.skb.cross_validatehtml.md#skrub.DataOp.skb.cross_validate)
: Perform cross-validation on a DataOp.

[`make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Perform hyperparameter tuning driven by cross-validation scores.

[`make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Perform hyperparameter tuning driven by cross-validation scores.

### Examples

```pycon
>>> import skrub
>>> from sklearn.dummy import DummyClassifier
>>> orders = skrub.var("orders", skrub.datasets.toy_orders().orders)
```

We want to predict whether orders are “delayed”:

```pycon
>>> X = orders.skb.drop("delayed").skb.mark_as_X()
>>> y = orders["delayed"].skb.mark_as_y()
>>> delayed = X.skb.apply(skrub.TableVectorizer()).skb.apply(
...     DummyClassifier(), y=y
... )
```

We can split the data into a training and test set with
`.skb.train_test_split()`. It it also possible to specify parameters
for the splitting function, such as the random state, or the size of the
test set. These parameters are passed as keyword arguments to
`train_test_split`, which then passes them to the splitting function.

```pycon
>>> split = delayed.skb.train_test_split(random_state=0, test_size=0.2)
>>> split.keys()
dict_keys(['X_train', 'X_test', 'y_train', 'y_test', 'train', 'test', 'X', 'y'])
>>> learner = delayed.skb.make_learner()
>>> learner.fit(split["train"])
SkrubLearner(data_op=<Apply DummyClassifier>)
>>> learner.score(split["test"])
0.0
```

The test split can then be used to evaluate the learner with any metric,
for example accuracy:

```pycon
>>> from sklearn.metrics import accuracy_score
>>> predictions = learner.predict(split["test"])
>>> accuracy_score(split["y_test"], predictions)
0.0
```

If [`mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) was defined to use a specific
cross-validation splitter, that splitter will be used by `train_test_split`,
which will return a split produced by the splitter. By default it is
the last; that can be controlled by passing `split_index`.

```pycon
>>> from sklearn.model_selection import LeaveOneOut
>>> X = orders.skb.drop("delayed").skb.mark_as_X(cv=LeaveOneOut())
>>> y = orders["delayed"].skb.mark_as_y()
>>> delayed = X.skb.apply(skrub.TableVectorizer()).skb.apply(
...     DummyClassifier(), y=y
... )
>>> split = delayed.skb.train_test_split()
>>> split["y_test"]
3    False
Name: delayed, dtype: bool
```

```pycon
>>> split = delayed.skb.train_test_split(split_index=2)
>>> split["y_test"]
2    True
Name: delayed, dtype: bool
```

Note that if a `split_func` is passed to `train_test_split`, it
overrides `mark_as_X`: the splitter defined in `mark_as_X` is
ignored, and the function passed to `train_test_split` is used instead.

<!-- !! processed by numpydoc !! -->
