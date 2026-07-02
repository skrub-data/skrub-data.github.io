# boolean

### skrub.selectors.boolean()

Select columns that have a Boolean data type.

#### SEE ALSO
[`numeric`](skrub.selectors.numerichtml.md#skrub.selectors.numeric)
: Select all numeric columns (integer and float).

[`integer`](skrub.selectors.integerhtml.md#skrub.selectors.integer)
: Select integer columns.

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
>>> df.dtypes
i64        int64
i8          int8
bool_       bool
Bool_    boolean
dtype: object
```

```pycon
>>> s.select(df, s.boolean())
   bool_  Bool_
0   True  False
```

<!-- !! processed by numpydoc !! -->
