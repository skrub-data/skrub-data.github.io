# glob

### skrub.selectors.glob(pattern)

Select columns by name with Unix shell style ‘glob’ pattern.

Pattern matching is case-sensitive and interpreted as described in
`fnmatch.fnmatchcase`:

```default
*       matches everything
?       matches any single character
[seq]   matches any character in seq
[!seq]  matches any char not in seq
```

* **Parameters:**
  **pattern**
  : A glob pattern to match column names.

#### SEE ALSO
[`regex`](skrub.selectors.regexhtml.md#skrub.selectors.regex)
: Select columns by name using regular expressions. Use this for complex patterns that glob cannot express.

[`filter_names`](skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names)
: Select columns based on custom name-based criteria.

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
```

Select columns matching a pattern:

```pycon
>>> s.select(df, s.glob('*_mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

Use character classes to match specific patterns:

```pycon
>>> s.select(df, s.glob('[a-z]*_mm'))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

Combine with other selectors:

```pycon
>>> s.select(df, s.glob('*_mm') | s.glob('ID'))
   height_mm  width_mm  ID
0      297.0     210.0   4
1      420.0     297.0   3
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
