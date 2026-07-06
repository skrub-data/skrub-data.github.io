# numeric

### skrub.selectors.numeric()

Select columns that have a numeric data type.

This selects float and integer columns but not Boolean columns.

#### SEE ALSO
[`integer`](skrub.selectors.integerhtml.md#skrub.selectors.integer)
: Select integer columns.

[`float`](skrub.selectors.floathtml.md#skrub.selectors.float)
: Select floating-point columns.

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
...         i64=[2],
...         I64=pd.Series([2]).convert_dtypes(),
...         i8=np.int8(3),
...         bool_=[True],
...         Bool_=pd.Series([True]).convert_dtypes(),
...         str_=["hello"],
...     )
... )
```

```pycon
>>> df
   f64  F64  i64  I64  i8  bool_  Bool_   str_
0  1.1  2.3    2    2   3   True   True  hello
>>> df.dtypes
f64      float64
F64      Float64
i64        int64
I64        Int64
i8          int8
bool_       bool
Bool_    boolean
str_      ...
dtype: object
```

```pycon
>>> s.select(df, s.numeric())
   f64  F64  i64  I64  i8
0  1.1  2.3    2    2   3
```

Use s.boolean() to also select Boolean columns:

```pycon
>>> s.select(df, s.numeric() | s.boolean())
   f64  F64  i64  I64  i8  bool_  Bool_
0  1.1  2.3    2    2   3   True   True
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
