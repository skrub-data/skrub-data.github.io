# skrub.DataOp.skb.concat

#### DataOp.skb.concat(others, axis=0)

Concatenate arrays or dataframes vertically or horizontally.

* **Parameters:**
  **others**
  : The arrays or dataframes to stack with `self`. Must be of the
    same type as `self`: do not mix arrays with dataframes.

  **axis**
  : The axis to concatenate along.
    0: stack vertically (rows)
    1: stack horizontally (columns)
* **Returns:**
  [`array`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.html#numpy.ndarray) or dataframe
  : The combined arrays or dataframes.

### Examples

```pycon
>>> import pandas as pd
>>> import skrub
>>> a = skrub.var('a', pd.DataFrame({'a1': [0], 'a2': [1]}))
>>> b = skrub.var('b', pd.DataFrame({'b1': [2], 'b2': [3]}))
>>> c = skrub.var('c', pd.DataFrame({'c1': [4], 'c2': [5]}))
>>> d = skrub.var('d', pd.DataFrame({'c1': [6], 'c2': [7]}))
>>> e = skrub.var('e', pd.DataFrame({'c1': [8], 'c2': [9]}))
>>> a
<Var 'a'>
Result:
―――――――
   a1  a2
0   0   1
>>> a.skb.concat([b, c], axis=1)
<Concat: 3 tables>
Result:
―――――――
   a1  a2  b1  b2  c1  c2
0   0   1   2   3   4   5
```

```pycon
>>> c.skb.concat([d, e], axis=0)
<Concat: 3 tables>
Result:
―――――――
   c1  c2
0   4   5
1   6   7
2   8   9
```

Note that even if we want to concatenate a single dataframe we must
still put it in a list:

```pycon
>>> a.skb.concat([b], axis=1)
<Concat: 2 tables>
Result:
―――――――
   a1  a2  b1  b2
0   0   1   2   3
```

<!-- !! processed by numpydoc !! -->
