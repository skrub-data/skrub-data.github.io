# skrub.DataOp.skb.get_data

#### DataOp.skb.get_data()

Collect the values of the variables contained in the DataOp.

* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict) mapping variable names to their values
  : Variables for which no value was given do not appear in the result.

#### SEE ALSO
[`DataOp.skb.get_vars`](skrub.DataOp.skb.get_varshtml.md#skrub.DataOp.skb.get_vars)
: Obtain the variables (the `skrub.var()` objects) themselves.

[`DataOp.skb.set_data`](skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data)
: Set new values for the variables.

### Examples

```pycon
>>> import skrub
>>> a = skrub.var('a', 0)
>>> b = skrub.var('b', 1)
>>> c = skrub.var('c') # note no value
>>> e = a + b + c
>>> e.skb.get_data()
{'a': 0, 'b': 1}
```

<!-- !! processed by numpydoc !! -->
