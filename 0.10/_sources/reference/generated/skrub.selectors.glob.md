# glob

### skrub.selectors.glob(pattern)

Select columns by name with Unix shell style ‘glob’ pattern.

pattern is interpreted as described in `fnmatch.fnmatchcase`:

```default
*       matches everything
?       matches any single character
[seq]   matches any character in seq
[!seq]  matches any char not in seq
```

#### SEE ALSO
[`regex`](skrub.selectors.regexhtml.md#skrub.selectors.regex)
: Select columns by name using a regular expression.

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
>>> s.select(df, s.glob('*'))
   height_mm  width_mm kind  ID
0      297.0     210.0   A4   4
1      420.0     297.0   A3   3
>>> s.select(df, s.glob('*_mm'))
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
