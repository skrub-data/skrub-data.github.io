# skrub.DataOp.skb.freeze_after_fit

#### DataOp.skb.freeze_after_fit()

Freeze the result during learner fitting.

With `freeze_after_fit()` the result of the DataOp is
computed during `fit()`, and then reused (not recomputed) during
`transform()` or `predict()`.

* **Returns:**
  The DataOp whose value does not change after `fit()`

### Notes

This is an advanced functionality, and the need for it is usually
an indication that we need to define a custom scikit-learn transformer
that we can use with `.skb.apply()`.

### Examples

```pycon
>>> import skrub
>>> X_df = skrub.datasets.toy_orders().X
>>> X_df
   ID product  quantity        date
0   1     pen         2  2020-04-03
1   2     cup         3  2020-04-04
2   3     cup         5  2020-04-04
3   4   spoon         1  2020-04-05
>>> n_products = skrub.X()['product'].nunique()
>>> transformer = n_products.skb.make_learner()
>>> transformer.fit_transform({'X': X_df})
3
```

If we take only the first 2 rows `nunique()` (a stateless function)
returns `2`:

```pycon
>>> transformer.transform({'X': X_df.iloc[:2]})
2
```

If instead of recomputing it we want the number of products to be
remembered during `fit` and reused during `transform`:

```pycon
>>> n_products = skrub.X()['product'].nunique().skb.freeze_after_fit()
>>> transformer = n_products.skb.make_learner()
>>> transformer.fit_transform({'X': X_df})
3
>>> transformer.transform({'X': X_df.iloc[:2]})
3
```

<!-- !! processed by numpydoc !! -->
