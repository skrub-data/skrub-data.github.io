# skrub.DataOp.skb.set_data

#### DataOp.skb.set_data(data)

Get a new DataOp with the provided preview values set on the variables.

* **Returns:**
  DataOp
  : A clone of the original DataOp, with the values in `data` used
    as the preview values for the corresponding variables.

#### SEE ALSO
[`DataOp.skb.get_data`](skrub.DataOp.skb.get_datahtml.md#skrub.DataOp.skb.get_data)
: Obtain the preview values currently set on the variables.

[`DataOp.skb.get_vars`](skrub.DataOp.skb.get_varshtml.md#skrub.DataOp.skb.get_vars)
: Obtain the variables (the `skrub.var()` objects) themselves.

[`DataOp.skb.clone`](skrub.DataOp.skb.clonehtml.md#skrub.DataOp.skb.clone)
: Obtain an independent clone of the DataOp, which does not contain any computed preview results. The parameter `drop_values` controls whether the values set on variables (if any) should be kept.

### Examples

```pycon
>>> import skrub
>>> a = skrub.var('a')
>>> b = skrub.var('b')
>>> c = a + b
```

We have initialized our variables without any value, there is no preview
result available for our DataOp `c`:

```pycon
>>> c
<BinOp: add>
>>> c * 2
<BinOp: mul>
```

As usual, we can evaluate `c`, passing values for the variables it
contains:

```pycon
>>> c.skb.eval({'a': 1, 'b': 2})
3
```

But suppose we want to start working in a more interactive fashion and would
benefit from preview computations that run whenever we use our DataOp, without
needing to explicitly call `eval`. We can inject values into the existing
DataOp.

```pycon
>>> d = c.skb.set_data({'a': 1, 'b': 2})
>>> d
<BinOp: add>
Result:
―――――――
3
>>> d * 2
<BinOp: mul>
Result:
―――――――
6
```

(Note that `set_data` returns a new DataOp (`d`) and leaves the
original one (`c`) unchanged.)

DataOps that use `d` can compute previews of the results because unlike `c`,
`d` has values for its variables:

```pycon
>>> c.skb.get_data()
{}
>>> d.skb.get_data()
{'a': 1, 'b': 2}
```

If the DataOp already contained preview values, they are replaced by
the provided ones. If we pass values for only some of the variables,
any already-existing values for the other variables are kept:

```pycon
>>> e = d.skb.set_data({'a': 10})
>>> e.skb.get_data()
{'a': 10, 'b': 2}
>>> e
<BinOp: add>
Result:
―――――――
12
```

If we want to drop the values attached to variables we can use
[`DataOp.skb.clone()`](skrub.DataOp.skb.clonehtml.md#skrub.DataOp.skb.clone). By default it drops values (we can pass
`drop_values=False` to prevent that).

```pycon
>>> f = e.skb.clone()
>>> f
<BinOp: add>
>>> f.skb.get_data()
{}
```

When we call `set_data`, passing keys in `data` that do not have a
corresponding variable in the DataOp is an error.

```pycon
>>> f.skb.set_data({'x': 0})
Traceback (most recent call last):
    ...
ValueError: The following keys were passed to set_data but have no corresponding variable in the DataOp: ['x']
```

<!-- !! processed by numpydoc !! -->
