# skrub.DataOp.skb.describe_steps

#### DataOp.skb.describe_steps()

Get a text representation of the computation graph.

Usually the graphical representation provided by [`DataOp.skb.draw_graph()`](skrub.DataOp.skb.draw_graphhtml.md#skrub.DataOp.skb.draw_graph)
or [`DataOp.skb.full_report()`](skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report) is more useful. This is a fallback for
inspecting the computation graph when only text output is available.

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : A string representing the different computation steps, one on each
    line.

#### SEE ALSO
[`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate)
: Evaluate metric(s) by cross-validation and also record fit/score times.

[`skrub.DataOp.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)
: Get a skrub learner for this DataOp.

### Examples

```pycon
>>> import skrub
>>> a = skrub.var('a')
>>> b = skrub.var('b')
>>> c = a + b
>>> d = c * c
>>> print(d.skb.describe_steps())
Var 'a'
Var 'b'
BinOp: add
( Var 'a' )*
( Var 'b' )*
( BinOp: add )*
BinOp: mul
* Cached, not recomputed
```

The above should be read from top to bottom as instructions for a
simple stack machine: load the variable ‘a’, load the variable ‘b’,
compute the addition leaving the result of (a + b) on the stack, then
repeat this operation (but the second time no computation actually runs
because the result of evaluating `c` has been cached in-memory), and
finally evaluate the multiplication.

<!-- !! processed by numpydoc !! -->
