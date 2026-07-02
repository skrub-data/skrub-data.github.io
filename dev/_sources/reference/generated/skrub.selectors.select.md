# select

### skrub.selectors.select(df, selector)

Apply a selector to a dataframe and return a dataframe with the selected     columns.

`selector` can be anything accepted by `make_selector` i.e. a selector,
column name or list of column names.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     {
...         "height_mm": [297.0, 420.0],
...         "width_mm": [210.0, 297.0],
...         "kind": ["A4", "A3"],
...         "ID": [4, 3],
...     }
... )
>>> df
   height_mm  width_mm kind  ID
0      297.0     210.0   A4   4
1      420.0     297.0   A3   3
```

```pycon
>>> selector = s.all() - 'ID'
>>> selector
(all() - cols('ID'))
```

```pycon
>>> selector.expand(df)
['height_mm', 'width_mm', 'kind']
```

```pycon
>>> s.select(df, selector)
   height_mm  width_mm kind
0      297.0     210.0   A4
1      420.0     297.0   A3
```

We can also pass column names directly:

```pycon
>>> s.select(df, ['kind', 'ID'])
  kind  ID
0   A4   4
1   A3   3
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
