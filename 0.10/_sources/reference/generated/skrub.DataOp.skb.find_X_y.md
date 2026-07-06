# skrub.DataOp.skb.find_X_y

#### DataOp.skb.find_X_y()

Find the nodes that have been marked with `mark_as_X()` and `mark_as_y()`.

* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : A dictionary containing the following keys (all are optional):
    - “X”, if a node has been marked with [`DataOp.skb.mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X).
    - “y”, if a node has been marked with [`DataOp.skb.mark_as_y()`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y).
    - Additionally, if a `cv` has been passed to
      [`DataOp.skb.mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X), the parameters that were passed to
      `mark_as_X`:
      - “cv”
      - “split_kwargs”

#### SEE ALSO
[`DataOp.skb.find`](skrub.DataOp.skb.findhtml.md#skrub.DataOp.skb.find)
: Find a node by name or by an arbitrary predicate.

[`SkrubLearner.truncated_after`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.truncated_after)
: Truncate the (possibly fitted) SkrubLearner after the specified node.

### Notes

To evaluate the DataOps in the returned dictionary, it is recommended
to evaluate the whole dict as a single DataOp:

```default
Xy = my_data_op.skb.find_X_y()
Xy_values = skrub.as_data_op(Xy).skb.eval({...})
X_value = Xy_values['X']
y_value = Xy_values['y']
```

rather than:

```default
Xy = my_data_op.skb.find_X_y()
X_value = Xy['X'].skb.eval({...})
y_value = Xy['y'].skb.eval({...})
```

Indeed, evaluating each value in the dict separately can result in
running some computation twice, and worse, obtaining X and y that are
not aligned if the data loading and processing that produces X and y
produces row in an undeterministic order (e.g. due to aggregations,
joins, database queries etc.).

### Examples

```pycon
>>> import skrub
>>> from sklearn.dummy import DummyClassifier
```

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

```pycon
>>> data = skrub.var("df")
>>> groups = data["seller"]
>>> X = data[["description", "price"]].skb.mark_as_X()
>>> y = data["category"].skb.mark_as_y()
>>> pred = X.skb.apply(DummyClassifier(), y=y)
>>> X_y = pred.skb.find_X_y()
>>> X_y
{'X': <GetItem ['description', 'price']>, 'y': <GetItem 'category'>}
>>> X_y['X'] is X
True
```

To compute the values, evaluate the whole dictionary as a single DataOp:

```pycon
>>> X_y_values = skrub.as_data_op(X_y).skb.eval({'df': df})
>>> X_y_values
{'X':    description  price
0       screen    100
1       hammer     15
2     keyboard     20
3      usb key      9
4      charger     13
5  screwdriver     12, 'y': 0    electronics
1          tools
2    electronics
3    electronics
4    electronics
5          tools
Name: category, dtype: str}
```

When a `cv` object was passed to [`DataOp.skb.mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X), the result
will also contain the keys `"cv"` and `"split_kwargs"`:

```pycon
>>> from sklearn.model_selection import LeaveOneGroupOut
```

```pycon
>>> X = data[["description", "price"]].skb.mark_as_X(
...     cv=LeaveOneGroupOut(), split_kwargs={"groups": groups}
... )
>>> pred = X.skb.apply(DummyClassifier(), y=y)
>>> pred.skb.find_X_y()
{'X': <X>, 'cv': LeaveOneGroupOut(), 'split_kwargs': {'groups': <GetItem 'seller'>}, 'y': <GetItem 'category'>}
```

<!-- !! processed by numpydoc !! -->
