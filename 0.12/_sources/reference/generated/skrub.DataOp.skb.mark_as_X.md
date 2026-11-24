# skrub.DataOp.skb.mark_as_X

#### DataOp.skb.mark_as_X(, cv=None, split_kwargs=None)

Mark this DataOp as being the `X` table.

Returns a copy; the original DataOp is left unchanged.

This is used for train/test splits and cross-validation.
To create a split,

- The nodes that are marked as `X` and `y` are materialized: all
  the operations that come before are executed, to compute the value of
  `X` and `y`.
- Then the resulting tables are divided into train and test parts
  according to the splitting strategy.
- For each split, the rest of the DataOp is fitted on the train set and
  tested on the test set.

In addition, `mark_as_X` can be passed a splitter (`cv`) and named
arguments for the splitter. Those will be evaluated at the same time as
`X` and `y` and used to create the train/test splits.

* **Parameters:**
  **cv**
  : Cross-validation splitting strategy. It can be:
    - None: 5-fold (stratified) cross-validation
    - integer: specify the number of folds
    - sklearn [CV splitter](https://scikit-learn.org/stable/modules/cross_validation.html#cross-validation-iterators)
    - iterable yielding (train, test) splits as arrays of indices.

  **split_kwargs**
  : Named arguments for the `split()` function (such as groups for a
    [`GroupKFold`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GroupKFold.html#sklearn.model_selection.GroupKFold)). Note that
    `split_kwargs` (and `cv`) can themselves be DataOps.
* **Returns:**
  A new DataOp, which has been marked as being the `X` table (which

  must be split for cross-validation).

#### SEE ALSO
[`DataOp.skb.mark_as_y()`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y)
: The equivalent of this function for the targets: mark a node as being the `y` table.

[`skrub.X()`](skrub.Xhtml.md#skrub.X)
: `skrub.X(value)` can be used as a shorthand for `skrub.var('X', value).skb.mark_as_X()`.

[`DataOp.skb.train_test_split()`](skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split)
: Prepare training and testing sets for a DataOp.

[`DataOp.skb.cross_validate()`](skrub.DataOp.skb.cross_validatehtml.md#skrub.DataOp.skb.cross_validate)
: Perform cross-validation on a DataOp.

[`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Perform hyperparameter tuning driven by cross-validation scores.

[`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Perform hyperparameter tuning driven by cross-validation scores.

### Notes

During cross-validation, all the previous steps are first executed,
until X and y (and the splitter and its additional arguments, if any)
have been materialized. Then, X and y are split into training and
testing sets. The following steps in the DataOp are fitted on the train
data, and applied to test data, within each split.

This means that any step that comes before `mark_as_X()` or
`mark_as_y()`, meaning that it is needed to compute X and y, sees the
full dataset and cannot benefit from hyperparameter tuning. So we
should be careful to start our learner by building X and y, and to use
`mark_as_X()` and `mark_as_y()` as soon as possible.

`skrub.X(value)` can be used as a shorthand for
`skrub.var('X', value).skb.mark_as_X()`.

### Examples

```pycon
>>> import skrub
>>> orders = skrub.var('orders', skrub.datasets.toy_orders(split='all').orders)
>>> features = orders.drop(columns='delayed', errors='ignore')
>>> features.skb.is_X
False
>>> X = features.skb.mark_as_X()
>>> X.skb.is_X
True
```

Note the original is left unchanged

```pycon
>>> features.skb.is_X
False
```

```pycon
>>> y = orders['delayed'].skb.mark_as_y()
>>> y.skb.is_y
True
```

Now if we run cross-validation:

```pycon
>>> from sklearn.dummy import DummyClassifier
>>> pred = X.skb.apply(DummyClassifier(), y=y)
>>> pred.skb.cross_validate(cv=2)['test_score']
0    0.666667
1    0.666667
Name: test_score, dtype: float64
```

First (outside of the cross-validation loop) `X` and `y` are
computed. Then, they are split into training and test sets. Then the
rest of the learner (in this case the last step, the
`DummyClassifier`) is evaluated on those splits.

We can pass additional data to the cross-validation splitter by using
the `cv` and `split_kwargs` parameters:

```pycon
>>> df = skrub.datasets.toy_products()
>>> df
   description  price            seller     category
0       screen    100   supermarket.com  electronics
1       hammer     15  bestproducts.com        tools
2     keyboard     20   supermarket.com  electronics
3      usb key      9  bestproducts.com  electronics
4      charger     13  bestproducts.com  electronics
5  screwdriver     12   supermarket.com        tools
```

Suppose we want to assess generalization to new sellers. While splitting for
cross-validation we must group products by seller. We do it with
[`sklearn.model_selection.LeaveOneGroupOut`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.LeaveOneGroupOut.html#sklearn.model_selection.LeaveOneGroupOut).

```pycon
>>> from sklearn.dummy import DummyClassifier
>>> from sklearn.model_selection import LeaveOneGroupOut
```

```pycon
>>> data = skrub.var("df", df)
>>> groups = data["seller"]
>>> X = data[["description", "price"]].skb.mark_as_X(
...     cv=LeaveOneGroupOut(), split_kwargs={"groups": groups}
... )
>>> y = data["category"].skb.mark_as_y()
>>> pred = X.skb.apply(DummyClassifier(), y=y)
>>> split = pred.skb.train_test_split()
```

The train set only contains data from the “bestproducts.com” seller.

```pycon
>>> split["X_train"]
  description  price
1      hammer     15
3     usb key      9
4     charger     13
```

The test set only contains data from the “supermarket.com” seller.

```pycon
>>> split["X_test"]
   description  price
0       screen    100
2     keyboard     20
5  screwdriver     12
```

<!-- !! processed by numpydoc !! -->
