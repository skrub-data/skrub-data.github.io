# float

### skrub.selectors.float()

Select columns that have a floating-point data type (float32, float64, etc.)

#### SEE ALSO
[`numeric`](skrub.selectors.numerichtml.md#skrub.selectors.numeric)
: Select all numeric columns (integer and float). Use this to select both integer and floating-point columns together.

[`integer`](skrub.selectors.integerhtml.md#skrub.selectors.integer)
: Select integer columns only.

[`boolean`](skrub.selectors.booleanhtml.md#skrub.selectors.boolean)
: Select Boolean columns.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> import numpy as np
>>> df = pd.DataFrame(
...     dict(
...         f64=[1.1],
...         F64=pd.Series([2.3]).convert_dtypes(),
...         f32=np.asarray(3.4, dtype='float32'),
...         i64=[2],
...         I64=pd.Series([2]).convert_dtypes(),
...         i8=np.int8(3),
...         bool_=[True],
...         Bool_=pd.Series([True]).convert_dtypes(),
...         str_=["hello"],
...     )
... )
>>> df
   f64  F64  f32  i64  I64  i8  bool_  Bool_   str_
0  1.1  2.3  3.4    2    2   3   True   True  hello
>>> df.dtypes
f64      float64
F64      Float64
f32      float32
i64        int64
I64        Int64
i8          int8
bool_       bool
Bool_    boolean
str_      ...
dtype: object
```

Select all floating-point columns:

```pycon
>>> s.select(df, s.float())
   f64  F64  f32
0  1.1  2.3  3.4
```

Combine with other selectors:

```pycon
>>> s.select(df, s.float() | s.integer())
   f64  F64  f32  i64  I64  i8
0  1.1  2.3  3.4    2    2   3
```

<!-- !! processed by numpydoc !! -->
