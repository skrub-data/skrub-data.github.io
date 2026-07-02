# skrub.DataOp.skb.eval

#### DataOp.skb.eval(environment=None, , keep_subsampling=False)

Evaluate the DataOp.

This returns the result produced by evaluating the DataOp, ie
running the corresponding learner. The result is always the output
of the learner’s `fit_transform` – a learner is refitted to the
provided data.

If no data is provided, the values passed when creating the variables
in the DataOp are used.

* **Parameters:**
  **environment**
  : If `None`, the initial values of the variables contained in the
    DataOp are used. If a dict, it must map the name of each
    variable to a corresponding value.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), use a subsample of the data. By
    default subsampling is not applied and all the data is used.
* **Returns:**
  result
  : The result of running the computation, ie of executing the
    learner’s `fit_transform` on the provided data.

#### SEE ALSO
[`DataOp.skb.preview`](skrub.DataOp.skb.previewhtml.md#skrub.DataOp.skb.preview)
: Access the preview of the result on the variables initial values, with subsampling. Faster than `eval` but does not allow passing new data and always applies subsampling.

### Examples

We can define variables with initial values that can be used for evaluation:

```pycon
>>> import skrub
>>> a = skrub.var('a', 10)
>>> b = skrub.var('b', 5)
>>> c = a + b
>>> c
<BinOp: add>
Result:
―――――――
15
>>> c.skb.eval()
15
>>> c.skb.eval({'a': 1, 'b': 2})
3
```

<!-- !! processed by numpydoc !! -->
