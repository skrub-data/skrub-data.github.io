# filter_names

### skrub.selectors.filter_names(predicate, \*args, \*\*kwargs)

Select columns based only on their name.

The predicate takes as input the column name and must return True if it
should be selected and False otherwise.

* **Parameters:**
  **predicate**
  : A function that takes a column name (string) and optional extra arguments,
    returning `True` to select the column or `False` to exclude it.
    Signature: `predicate(col_name, *args, **kwargs) -> bool`

  **\*args**
  : Extra positional arguments passed to the predicate. Using explicit
    arguments (instead of closures) helps with pickling the selector.

  **\*\*kwargs**
  : Extra keyword arguments passed to the predicate. Using explicit
    arguments (instead of closures) helps with pickling the selector.
* **Returns:**
  Selector
  : A `NameFilter` selector that matches columns where
    `predicate(name, *args, **kwargs)` returns `True`.

#### SEE ALSO
[`filter`](skrub.selectors.filterhtml.md#skrub.selectors.filter)
: Select columns based on their content or properties

[`glob`](skrub.selectors.globhtml.md#skrub.selectors.glob)
: Select columns by wildcard pattern matching

[`regex`](skrub.selectors.regexhtml.md#skrub.selectors.regex)
: Select columns by regular expression pattern matching

### Notes

`filter_names` passes the column NAME
(a string), while `filter` passes the column OBJECT (Series/DataFrame)

For common patterns, prefer simpler selectors:

- `s.glob('*_mm')` instead of `s.filter_names(lambda n: n.endswith('_mm'))`
- `s.regex(r'col_\d+')` instead of `s.filter_names(lambda n: re.match(...))`

To pickle the selector, use importable functions as predicates
rather than lambdas. Pass parameters via `*args` or `**kwargs`:

```default
# Picklable (importable function + explicit args)
s.filter_names(str.endswith, '_mm')

# Picklable alternative (with functools.partial)
import functools
s.filter_names(functools.partial(str.endswith, '_mm'))

# NOT picklable (closure)
suffix = '_mm'
s.filter_names(lambda name: name.endswith(suffix))
```

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

Prefer using 

```
*
```

args and 

```
**
```

kwargs to pass extra arguments to the predicate,
rather than defining a dynamic function which may cause pickling errors.

Select columns ending with a suffix (using lambda):

```pycon
>>> selector = s.filter_names(lambda name: name.endswith('_mm'))
>>> s.select(df, selector)
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

Use an importable function with explicit arguments (picklable):

```pycon
>>> selector = s.filter_names(str.endswith, '_mm')
>>> selector
filter_names(str.endswith, '_mm')
```

```pycon
>>> s.select(df, selector)
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

```pycon
>>> import pickle
>>> _ = pickle.dumps(selector)  # Pickling works!
```

Select columns with uppercase names:

```pycon
>>> s.select(df, s.filter_names(str.isupper))
   ID
0   4
1   3
```

Combine with type selectors:

```pycon
>>> s.select(df, s.numeric() & s.filter_names(str.islower))
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

<!-- !! processed by numpydoc !! -->
