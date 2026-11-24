# skrub.DataOp.skb.set_description

#### DataOp.skb.set_description(description)

Give a description to this DataOp.

Returns a modified copy.

The description can help document our learner. It is displayed in the
execution report and can be retrieved from the [`DataOp.skb.description`](skrub.DataOp.skb.descriptionhtml.md#skrub.DataOp.skb.description)
attribute.

* **Parameters:**
  **description**
  : The description
* **Returns:**
  A new DataOp with the provided description.

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
