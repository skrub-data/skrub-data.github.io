# skrub.DataOp.skb.applied_estimator

#### DataOp.skb.applied_estimator

Retrieve the estimator applied in the previous step, as a DataOp.

### Notes

This attribute only exists for DataOp created with
`.skb.apply()`.

### Examples

```pycon
>>> import skrub
>>> orders_df = skrub.datasets.toy_orders().X
>>> features = skrub.X(orders_df).skb.apply(skrub.TableVectorizer())
>>> fitted_vectorizer = features.skb.applied_estimator
>>> fitted_vectorizer
<AppliedEstimator>
Result:
―――――――
ApplyToCols(transformer=TableVectorizer())
```

Note that in order to restrict transformers to a subset of columns,
they will be wrapped in a meta-estimator [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols). The
actual transformer can be retrieved through the `transformer_` (or
`transformers_`, in the case of single-column transformers); see the
documentation of [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols) for details.

```pycon
>>> fitted_vectorizer.transformer_
<GetAttr 'transformer_'>
Result:
―――――――
TableVectorizer()
```

```pycon
>>> fitted_vectorizer.transformer_.column_to_kind_
<GetAttr 'column_to_kind_'>
Result:
―――――――
{'ID': 'numeric', 'quantity': 'numeric', 'date': 'datetime', 'product': 'low_cardinality'}
```

Here is an example of an estimator applied column-wise:

```pycon
>>> orders_df['description'] = [f'describe {p}' for p in orders_df['product']]
>>> from skrub import selectors as s
>>> out = skrub.X(orders_df).skb.apply(
...     skrub.StringEncoder(n_components=2), cols=s.string() - "date"
... )
>>> fitted_vectorizer = out.skb.applied_estimator
>>> fitted_vectorizer
<AppliedEstimator>
Result:
―――――――
ApplyToCols(cols=(string() - cols('date')),
            transformer=StringEncoder(n_components=2))
>>> fitted_vectorizer.transformers_
<GetAttr 'transformers_'>
Result:
―――――――
{'product': StringEncoder(n_components=2), 'description': StringEncoder(n_components=2)}
```

<!-- !! processed by numpydoc !! -->
