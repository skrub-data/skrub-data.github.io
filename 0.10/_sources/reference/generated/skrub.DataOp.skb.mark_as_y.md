# skrub.DataOp.skb.mark_as_y

#### DataOp.skb.mark_as_y()

Mark this DataOp as being the `y` table.

This is used for cross-validation and hyperparameter selection: operations
done before [`skb.mark_as_X()`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) and [`skb.mark_as_y()`](#skrub.DataOp.skb.mark_as_y) are executed
on the entire data and cannot benefit from hyperparameter tuning.
Returns a copy; the original DataOp is left unchanged.

* **Returns:**
  The input DataOp, which has been marked as being `y`

#### SEE ALSO
[`skrub.y()`](skrub.yhtml.md#skrub.y)
: `skrub.y(value)` can be used as a shorthand for `skrub.var('y', value).skb.mark_as_y()`.

### Notes

During cross-validation, all the previous steps are first executed,
until X and y have been materialized. Then, those are split into
training and testing sets. The following steps in the DataOp plan are
fitted on the train data, and applied to test data, within each split.

This means that any step that comes before `mark_as_X()` or
`mark_as_y()`, meaning that it is needed to compute X and y, sees the
full dataset and cannot benefit from hyperparameter tuning. So we
should be careful to start our learner by building X and y, and to use
`mark_as_X()` and `mark_as_y()` as soon as possible.

### Examples

```pycon
>>> import skrub
>>> orders = skrub.var('orders', skrub.datasets.toy_orders(split='all').orders)
>>> X = orders.drop(columns='delayed', errors='ignore').skb.mark_as_X()
>>> delayed = orders['delayed']
>>> delayed.skb.is_y
False
>>> y = delayed.skb.mark_as_y()
>>> y.skb.is_y
True
```

Note the original is left unchanged

```pycon
>>> delayed.skb.is_y
False
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

Please see the examples gallery for more information.

<!-- !! processed by numpydoc !! -->
