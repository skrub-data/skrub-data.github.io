# skrub.DataOp.skb.description

#### DataOp.skb.description

A user-defined description or comment about the DataOp.

This can be set with [`DataOp.skb.set_description()`](skrub.DataOp.skb.set_descriptionhtml.md#skrub.DataOp.skb.set_description) and is displayed
in the execution report generated with [`full_report()`](skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report)
or [`report()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.report).

### Examples

```pycon
>>> import skrub
>>> a = skrub.var('a', 1)
>>> b = skrub.var('b', 2)
>>> c = (a + b).skb.set_description('the addition of a and b')
>>> c
<BinOp: add>
Result:
―――――――
3
>>> c.skb.description
'the addition of a and b'
```

<!-- !! processed by numpydoc !! -->
