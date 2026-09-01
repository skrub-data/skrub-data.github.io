# select

### skrub.selectors.select(df, selector)

Select the columns of a dataframe that are matched by the selector.

This function returns a new dataframe containing only the columns of `df`
matched by `selector`.

* **Parameters:**
  **df**
  : The dataframe to select columns from (pandas or polars).

  **selector**
  : A selector object, single column name, or list of column names.
* **Returns:**
  dataframe
  : A new dataframe containing only the columns matched by the selector.

#### SEE ALSO
[`drop`](skrub.selectors.drophtml.md#skrub.selectors.drop)
: Return all columns except those matched by a selector

[`Selector.expand`](skrub.selectors.Selectorhtml.md#skrub.selectors.Selector.expand)
: Get the column names matched by a selector as a list

### Notes

`select` is a convenience function that combines two operations:

1. `selector.expand(df)` - Get list of matching column names
2. Return the dataframe subset to those columns

If you only need the list of matching column names (without subsetting the
dataframe), use `selector.expand(df)` directly.

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

Select all columns except ‘ID’:

```pycon
>>> selector = s.all() - 'ID'
>>> selector
(all() - cols('ID'))
```

```pycon
>>> s.select(df, selector)
   height_mm  width_mm kind
0      297.0     210.0   A4
1      420.0     297.0   A3
```

Pass column names directly:

```pycon
>>> s.select(df, ['kind', 'ID'])
  kind  ID
0   A4   4
1   A3   3
```

Select by dtype:

```pycon
>>> s.select(df, s.numeric())
   height_mm  width_mm  ID
0      297.0     210.0   4
1      420.0     297.0   3
```

Combine multiple selectors:

```pycon
>>> s.select(df, s.numeric() & s.glob('*_mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
