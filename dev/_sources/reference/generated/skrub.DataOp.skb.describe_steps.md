# skrub.DataOp.skb.describe_steps

#### DataOp.skb.describe_steps()

Get a text representation of the computation graph.

Usually the graphical representation provided by [`DataOp.skb.draw_graph()`](skrub.DataOp.skb.draw_graph.md#skrub.DataOp.skb.draw_graph)
or [`DataOp.skb.report()`](skrub.DataOp.skb.report.md#skrub.DataOp.skb.report) is more useful. This is a fallback for
inspecting the computation graph when only text output is available.

* **Returns:**
  `python:str`
  : A string representing the different computation steps, one on each
    line.

#### SEE ALSO
[`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate)
: Evaluate metric(s) by cross-validation and also record fit/score times.

[`skrub.DataOp.skb.make_learner()`](skrub.DataOp.skb.make_learner.md#skrub.DataOp.skb.make_learner)
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
BinOp: add -> _2
Load _2 (BinOp: add)
BinOp: mul
```

The above should be read from top to bottom as instructions for a
simple stack machine: load the variable ‘a’, load the variable ‘b’,
compute the addition leaving the result of (a + b) on the stack, then
load the previous result again (the result of evaluating `c` has been
cached in-memory), and finally evaluate the multiplication.

As we can see results that are used several times are kept and not
re-computed; this is indicated in the printed list above by `-> _2`
(storing, where 2 is an arbitrary id / memory location) and `Load _2`
when reusing that result later.

<!-- !! processed by numpydoc !! -->
