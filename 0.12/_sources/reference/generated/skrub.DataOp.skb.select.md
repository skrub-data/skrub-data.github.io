# skrub.DataOp.skb.select

#### DataOp.skb.select(cols)

Select a subset of columns.

`cols` can be a column name, a list of column names, or a
[skrub selector](../selectorshtml.md#selectors-ref). Importantly, the exact list of
columns that match `.skb.select` is stored during `fit` and then this
same list of columns is selected during `transform`.

* **Parameters:**
  **cols**
  : The columns to select
* **Returns:**
  dataframe with only the selected columns

### Examples

```pycon
>>> import skrub
>>> from skrub import selectors as s
>>> X = skrub.X(skrub.datasets.toy_orders().X)
>>> X
<Var 'X'>
Result:
―――――――
   ID product  quantity        date
0   1     pen         2  2020-04-03
1   2     cup         3  2020-04-04
2   3     cup         5  2020-04-04
3   4   spoon         1  2020-04-05
>>> X.skb.select(['product', 'quantity'])
<Apply SelectCols>
Result:
―――――――
  product  quantity
0     pen         2
1     cup         3
2     cup         5
3   spoon         1
>>> X.skb.select(s.string())
<Apply SelectCols>
Result:
―――――――
  product        date
0     pen  2020-04-03
1     cup  2020-04-04
2     cup  2020-04-04
3   spoon  2020-04-05
```

<!-- !! processed by numpydoc !! -->
