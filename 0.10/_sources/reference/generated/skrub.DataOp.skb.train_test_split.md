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
  : The function used to split X and y once they have been computed. By
    default, [`train_test_split()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html#sklearn.model_selection.train_test_split) is used.

  **split_func_kwargs**
  : Additional named arguments to pass to the splitting function.
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : The return value is slightly different than scikit-learn’s. Rather than
    a tuple, it returns a dictionary with the following keys:
    - train: a dictionary containing the training environment
    - test: a dictionary containing the test environment
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
dict_keys(['train', 'test', 'X_train', 'X_test', 'y_train', 'y_test'])
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
splitting function, the splitter will be used by `train_test_split`.

Note that if a `cv` is passed to `train_test_split`, the splitter
defined in `mark_as_X` is ignored, and the one passed to `train_test_split`
is used instead.

```pycon
>>> from sklearn.model_selection import LeaveOneOut
>>> X = orders.skb.drop("delayed").skb.mark_as_X(cv=LeaveOneOut())
>>> y = orders["delayed"].skb.mark_as_y()
>>> delayed = X.skb.apply(skrub.TableVectorizer()).skb.apply(
...     DummyClassifier(), y=y
... )
>>> split = delayed.skb.train_test_split()
>>> split["y_test"]
0    False
Name: delayed, dtype: bool
```

<!-- !! processed by numpydoc !! -->
