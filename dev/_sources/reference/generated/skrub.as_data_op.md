# as_data_op

### skrub.as_data_op(value)

Create a DataOp [`DataOp`](skrub.DataOphtml.md#skrub.DataOp) that evaluates to the given value.

This wraps any object in a DataOp. When the DataOp is evaluated,
the result is the provided value. This has a similar role as [`deferred()`](skrub.deferredhtml.md#skrub.deferred),
but for any object rather than for functions.

* **Parameters:**
  **value**
  : The result of evaluating the DataOp
* **Returns:**
  a DataOp that evaluates to the given value

#### SEE ALSO
[`deferred`](skrub.deferredhtml.md#skrub.deferred)
: Wrap function calls in a DataOp.

[`DataOp`](skrub.DataOphtml.md#skrub.DataOp)
: Representation of a computation that can be used to build ML estimators.

### Examples

```pycon
>>> import skrub
>>> data_source = skrub.var('source')
>>> data_path = skrub.as_data_op(
...     {"local": "data.parquet", "remote": "remote/data.parquet"}
... )[data_source]
>>> data_path.skb.eval({'source': 'remote'})
'remote/data.parquet'
```

Turning the dictionary into a DataOp defers the lookup of
`data_source` until it has been evaluated when the learner runs.

The example above is somewhat contrived, but `as_data_op` is often useful
with choices.

```pycon
>>> x1 = skrub.var('x1')
>>> x2 = skrub.var('x2')
>>> features = skrub.choose_from({'x1': x1, 'x2': x2}, name='features')
>>> skrub.as_data_op(features).skb.apply(skrub.TableVectorizer())
<Apply TableVectorizer>
```

In fact, this can even be shortened slightly by using the choice’s method
`as_data_op`:

```pycon
>>> features.as_data_op().skb.apply(skrub.TableVectorizer())
<Apply TableVectorizer>
```

<!-- !! processed by numpydoc !! -->
