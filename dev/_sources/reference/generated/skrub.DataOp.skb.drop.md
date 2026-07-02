# skrub.DataOp.skb.drop

#### DataOp.skb.drop(cols)

Drop some columns.

`cols` can be a column name, a list of column names, or a
[skrub selector](../selectorshtml.md#selectors-ref). Importantly, the exact list of
columns that match `.skb.drop` is stored during `fit` and then this
same list of columns is dropped during `transform`.

* **Parameters:**
  **cols**
  : The columns to select
* **Returns:**
  dataframe without the dropped columns

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
>>> X.skb.drop(['ID', 'date'])
<Apply DropCols>
Result:
―――――――
  product  quantity
0     pen         2
1     cup         3
2     cup         5
3   spoon         1
>>> X.skb.drop(s.string())
<Apply DropCols>
Result:
―――――――
   ID  quantity
0   1         2
1   2         3
2   3         5
3   4         1
```

<!-- !! processed by numpydoc !! -->
