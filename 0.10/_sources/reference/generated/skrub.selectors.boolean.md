# boolean

### skrub.selectors.boolean()

Select columns that have a Boolean data type.

#### SEE ALSO
[`numeric`](skrub.selectors.numerichtml.md#skrub.selectors.numeric)
: Select all numeric columns (integer and float, NOT boolean).

[`integer`](skrub.selectors.integerhtml.md#skrub.selectors.integer)
: Select integer columns.

[`filter`](skrub.selectors.filterhtml.md#skrub.selectors.filter)
: Use for custom data-based selection criteria.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> import numpy as np
>>> df = pd.DataFrame(
...     dict(
...         i64=[0],
...         i8=np.int8(3),
...         bool_=[True],
...         Bool_=pd.Series([False]).convert_dtypes(),
...     )
... )
>>> df
   i64  i8  bool_  Bool_
0    0   3   True  False
```

Select all Boolean columns:

```pycon
>>> s.select(df, s.boolean())
   bool_  Bool_
0   True  False
```

Combine with numeric() to include both:

```pycon
>>> s.select(df, s.boolean() | s.numeric())
   i64  i8  bool_  Bool_
0    0   3   True  False
```

Note that numeric() alone does NOT include Boolean columns:

```pycon
>>> s.select(df, s.numeric())
   i64  i8
0    0   3
```

<!-- !! processed by numpydoc !! -->
