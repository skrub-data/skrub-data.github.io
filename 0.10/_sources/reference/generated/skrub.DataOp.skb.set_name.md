# skrub.DataOp.skb.set_name

#### DataOp.skb.set_name(name)

Give a name to this DataOp.

Returns a modified copy.

The name is displayed in the graph and reports so this can be useful to
mark relevant parts of the learner.

Moreover, the evaluation of this step can be bypassed and the result
provided directly by providing a value for this name to
[`eval()`](skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval), `.transform()`,
`.predict()` etc. (see examples)

* **Parameters:**
  **name**
  : The name for this step. Must be unique within a learner. Cannot
    start with `"_skrub_"`.
* **Returns:**
  A new DataOp with the given name.

### Examples

```pycon
>>> import skrub
>>> a = skrub.var('a', 1)
>>> b = skrub.var('b', 2)
>>> c = (a + b).skb.set_name('c')
>>> c
<c | BinOp: add>
Result:
―――――――
3
>>> c.skb.name
'c'
>>> d = c * 10
>>> d
<BinOp: mul>
Result:
―――――――
30
>>> d.skb.eval()
30
>>> d.skb.eval({'a': 10, 'b': 5})
150
```

We can override the result of `c`. When we do, the operands `a` and
`b` are not evaluated: evaluating `c` just returns the value we
passed.

```pycon
>>> d.skb.eval({'c': -1}) # -1 * 10
-10
```

For DataOps that are not variables, the name can be set back to the
default `None`:

```pycon
>>> e = c.skb.set_name(None)
>>> e.skb.name
>>> c.skb.name
'c'
```

<!-- !! processed by numpydoc !! -->
