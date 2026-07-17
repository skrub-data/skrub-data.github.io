# filter

### skrub.selectors.filter(predicate, \*args, \*\*kwargs)

Select columns for which a custom predicate function returns `True`.

This allows selecting columns based on an arbitrary criterion. The predicate takes
as input the column (a pandas or polars Series) and must return True if it
should be selected and False otherwise.

For example, this selector can be used when built-in selectors don’t match
your selection criterion, to select columns based on their content or computed
properties (e.g., variance, range…), or to combine multiple criteria in a
single predicate.

* **Parameters:**
  **predicate**
  : A function that takes a column and optional extra arguments, returning
    `True` to select the column or `False` to exclude it.
    Signature: `predicate(col, *args, **kwargs) -> bool`

  **\*args**
  : Extra positional arguments passed to the predicate.

  **\*\*kwargs**
  : Extra keyword arguments passed to the predicate.
* **Returns:**
  Selector
  : A `Filter` selector that matches columns where `predicate` returns `True`.

#### SEE ALSO
[`filter_names`](skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names)
: Select columns based only on their name (not content)

### Notes

For a column `col`, the predicate is called as:

```default
predicate(col, *args, **kwargs)
```

The column is kept if `predicate` returns `True`.

To pickle the selector, use importable functions as predicates
rather than lambdas. Prefer passing parameters via `*args` or `**kwargs`:

```default
# Picklable (importable function + explicit args)
s.filter(str.startswith, 'prefix')

# Picklable alternative (with functools.partial)
import functools
s.filter(functools.partial(str.startswith, 'prefix'))

# NOT picklable (closure)
prefix = 'test'
s.filter(lambda col: str(col.name).startswith(prefix))
```

For name-based selection, consider using [`filter_names()`](skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names), [`glob()`](skrub.selectors.globhtml.md#skrub.selectors.glob),
or [`regex()`](skrub.selectors.regexhtml.md#skrub.selectors.regex) instead of `filter` for simpler selection.

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

Select columns containing a specific value:

```pycon
>>> selector = s.filter(lambda col: 'A4' in col.values)
>>> s.select(df, selector)
  kind
0   A4
1   A3
```

Use an importable function with explicit arguments (picklable):

```pycon
>>> def contains(col, value):
...    return value in col.values
```

```pycon
>>> selector = s.filter(contains, 3)
>>> selector
filter(contains, 3)
```

```pycon
>>> s.select(df, selector)
   ID
0   4
1   3
```

Combine with type selectors:

```pycon
>>> def has_high_range(col):
...     return col.max() - col.min() > 100
```

```pycon
>>> s.select(df, s.numeric() & s.filter(has_high_range))
   height_mm
0      297.0
1      420.0
```

Note that `s.numeric()` short-circuits the evaluation of `has_high_range`
for non-numeric columns, so the predicate is only called for numeric columns,
avoiding errors on string columns.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
