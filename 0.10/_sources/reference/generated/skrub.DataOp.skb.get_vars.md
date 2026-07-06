# skrub.DataOp.skb.get_vars

#### DataOp.skb.get_vars(all_named_ops=False)

Get all the variables used in the DataOp.

* **Parameters:**
  **all_named_ops**
  : If `False`, return only actual variables (DataOps created with
    [`var()`](skrub.varhtml.md#skrub.var), [`X()`](skrub.Xhtml.md#skrub.X) or [`y()`](skrub.yhtml.md#skrub.y)). If `True`, return all
    nodes that have a name (i.e., for which a value can be passed in the
    environment).
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : Keys are names, and values the corresponding DataOp.

#### SEE ALSO
[`DataOp.skb.get_data`](skrub.DataOp.skb.get_datahtml.md#skrub.DataOp.skb.get_data)
: Get the values of the variables contained in the DataOp.

[`DataOp.skb.set_data`](skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data)
: Set new values for the variables.

[`DataOp.skb.set_name`](skrub.DataOp.skb.set_namehtml.md#skrub.DataOp.skb.set_name)
: Assign a name to a DataOp.

### Examples

```pycon
>>> import skrub
```

```pycon
>>> a = skrub.var("a")
>>> b = skrub.var("b")
```

We assign a name to a DataOp that is not a variable with
[`set_name()`](skrub.DataOp.skb.set_namehtml.md#skrub.DataOp.skb.set_name):

```pycon
>>> c = (a + b).skb.set_name("c")
```

```pycon
>>> d = c + c
>>> d
<BinOp: add>
```

Our DataOp, `d`, contains 2 variables: “a” and “b”:

```pycon
>>> d.skb.get_vars()
{'a': <Var 'a'>, 'b': <Var 'b'>}
```

Those are the keys for which we need to provide values in the
environment when evaluating `d`:

```pycon
>>> d.skb.eval({"a": 10, "b": 3}) # (10 + 3) + (10 + 3) = 26
26
```

In addition, we set a name on the internal node `c`. It is not a
variable, and normally it is computed as `(a + b)`. But as it has a
name, we can override its output by passing a value for `c` in the
environment. When we do, the computation of `c` never happens (nor of
`a` or `b`, here, because they are only used to compute `c`) – it is
bypassed and the provided value is used instead.

```pycon
>>> d.skb.eval({"c": 7}) # 7 + 7 = 14
14
```

If we want `get_vars` to also list nodes like our example `c` which
have a name and can be passed in the environment, we pass
`all_named_ops=True`:

```pycon
>>> d.skb.get_vars(all_named_ops=True)
{'a': <Var 'a'>, 'b': <Var 'b'>, 'c': <c | BinOp: add>}
```

Note `get_vars` can be particularly useful when we have a learner
(e.g. loaded from a pickle file) and we want to check what inputs we
should pass to its methods such as `fit` and `transform`:

```pycon
>>> learner = d.skb.make_learner()
>>> list(learner.data_op.skb.get_vars().keys())
['a', 'b']
```

The output above tells us what keys the dict we pass to
`learner.fit()` should contain:

```pycon
>>> learner.fit({'a': 2, 'b': 3})
SkrubLearner(data_op=<BinOp: add>)
```

<!-- !! processed by numpydoc !! -->
