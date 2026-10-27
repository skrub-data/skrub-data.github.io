# skrub.DataOp.skb.preview

#### DataOp.skb.preview()

Get the value computed for previews (shown when printing the DataOp).

* **Returns:**
  preview result
  : The result of evaluating the DataOp on the data stored in its
    variables.

#### SEE ALSO
[`DataOp.skb.subsample`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)
: Specify how to subsample an intermediate result when computing previews.

[`DataOp.skb.eval`](skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval)
: Evaluate the DataOp. Unlike `preview`, we can pass new data rather than using the values that variables were initialized with, but results are not cached, and no subsampling takes place by default.

### Examples

```pycon
>>> import skrub
```

```pycon
>>> a = skrub.var('a', 1)
>>> b = skrub.var('b', 2)
>>> c = a + b
```

When we display a DataOp, we see a preview of the result (`3` in
this case):

```pycon
>>> c
<BinOp: add>
Result:
―――――――
3
```

If we want to actually access that value `3`, rather than just seeing
it displayed, we can use `.skb.preview()`:

```pycon
>>> c.skb.preview()
3
```

This is the actual number `3`, not a DataOp or just a display

```pycon
>>> type(c.skb.preview())
<class 'int'>
```

Accessing the preview is usually faster than calling `.skb.eval()`
because results are cached and subsampling is used by default.

<!-- !! processed by numpydoc !! -->
