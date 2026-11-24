# skrub.DataOp.skb.clone

#### DataOp.skb.clone(drop_values=True)

Get an independent clone of the DataOp.

* **Parameters:**
  **drop_values**
  : Whether to drop the initial values passed to [`skrub.var()`](skrub.varhtml.md#skrub.var).
    This is convenient for example to serialize DataOps without
    creating large files.
* **Returns:**
  clone
  : A new DataOp which does not share its state (such as fitted
    estimators) or cache with the original, and possibly without the
    variables’ values.

#### SEE ALSO
[`DataOp.skb.set_data`](skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data)
: Set the initial (preview) values for variables contained in the DataOp.

### Examples

```pycon
>>> import skrub
>>> c = skrub.var('a', 0) + skrub.var('b', 1)
>>> c
<BinOp: add>
Result:
―――――――
1
>>> c.skb.get_data()
{'a': 0, 'b': 1}
>>> clone = c.skb.clone()
>>> clone
<BinOp: add>
>>> clone.skb.get_data()
{}
```

We can ask to keep the variable values:

```pycon
>>> clone = c.skb.clone(drop_values=False)
>>> clone.skb.get_data()
{'a': 0, 'b': 1}
```

Note that in that case the cache used for previews is still cleared. So
if we want the preview we need to prime the new DataOp by
accessing the preview once (either directly or by adding more steps to it):

```pycon
>>> clone
<BinOp: add>
>>> clone.skb.preview()
1
>>> clone
<BinOp: add>
Result:
―――――――
1
```

<!-- !! processed by numpydoc !! -->
