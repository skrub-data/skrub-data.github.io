<a id="user-guide-documenting-data-ops-plan"></a>

# Documenting the DataOps plan with node names and descriptions

We can improve the readability of the DataOps plan by giving names and descriptions
to the nodes in the plan. This is done with [`.skb.set_name()`](../../../reference/generated/skrub.DataOp.skb.set_namehtml.md#skrub.DataOp.skb.set_name)
and [`.skb.set_description()`](../../../reference/generated/skrub.DataOp.skb.set_descriptionhtml.md#skrub.DataOp.skb.set_description).

```pycon
>>> import skrub
>>> a = skrub.var('a', 1)
>>> b = skrub.var('b', 2)
>>> c = (a + b).skb.set_description('the addition of a and b')
>>> c.skb.description
'the addition of a and b'
>>> d = c.skb.set_name('d')
>>> d.skb.name
'd'
```

Both names and descriptions can be used to mark relevant parts of the learner, and
they can be accessed from the computational graph and the plan report.

Additionally, names can be used to bypass the computation of a node and override its
result by passing it as a key in the `environment` dictionary.

```pycon
>>> e = d * 10
>>> e
<BinOp: mul>
Result:
―――――――
30
>>> e.skb.eval()
30
>>> e.skb.eval({'a': 10, 'b': 5})
150
>>> e.skb.eval({'d': -1}) # -1 * 10
-10
```

More info can be found in section [Using only a part of a DataOps plan](using_part_of_data_ops_planhtml.md#user-guide-data-ops-truncating-dataplan).
