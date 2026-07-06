<a id="selectors-details"></a>

# Selecting based on dtype or data properties

Selectors can filter columns based on different conditions.

[`all()`](../../reference/generated/skrub.selectors.allhtml.md#skrub.selectors.all) is a simple selector, especially useful for default
arguments since it keeps all columns:

```pycon
>>> import pandas as pd
>>> from skrub import SelectCols
>>> import skrub.selectors as s
>>> df = pd.DataFrame(
...     {
...         "height_mm": [297.0, 420.0],
...         "width_mm": [210.0, 297.0],
...         "kind": ["A4", "A3"],
...         "ID": [4, 3],
...     }
... )
>>> SelectCols(cols=s.all()).fit_transform(df)
   height_mm  width_mm kind  ID
0      297.0     210.0   A4   4
1      420.0     297.0   A3   3
```

Selectors can be combined with operators, for example if we wanted all columns
except the “mm” columns above:

```pycon
>>> SelectCols(s.all() - s.cols("height_mm", "width_mm")).fit_transform(df)
  kind  ID
0   A4   4
1   A3   3
```

This module provides several kinds of selectors, which allow to select columns by
name, data type, contents, or according to arbitrary user-provided rules.

```pycon
>>> SelectCols(s.numeric()).fit_transform(df)
   height_mm  width_mm  ID
0      297.0     210.0   4
1      420.0     297.0   3
```

Selectors can be inverted with `~`, or [`inv()`](../../reference/generated/skrub.selectors.invhtml.md#skrub.selectors.inv):

```pycon
>>> SelectCols(~s.numeric()).fit_transform(df)
  kind
0   A4
1   A3
```

```pycon
>>> SelectCols(s.inv(s.numeric())).fit_transform(df)
  kind
0   A4
1   A3
```

Selectors can work on the column names. For example, to select the columns that
end with `_mm` we can do:

```pycon
>>> SelectCols(s.glob('*_mm')).fit_transform(df)
   height_mm  width_mm
0      297.0     210.0
1      420.0     297.0
```

<br/>

# Categories of selectors

The selectors in this module can be categorized based on what aspect of the columns
they examine:

## Selectors based on column data types

- [`numeric()`](../../reference/generated/skrub.selectors.numerichtml.md#skrub.selectors.numeric): Select columns with numeric data types (float and integer)
- [`integer()`](../../reference/generated/skrub.selectors.integerhtml.md#skrub.selectors.integer): Select columns with integer data types
- [`float()`](../../reference/generated/skrub.selectors.floathtml.md#skrub.selectors.float): Select columns with floating-point data types
- [`has_dtype()`](../../reference/generated/skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype): Select columns whose dtype exactly matches one of the provided dtypes
- [`any_date()`](../../reference/generated/skrub.selectors.any_datehtml.md#skrub.selectors.any_date): Select columns with date or datetime data types
- [`categorical()`](../../reference/generated/skrub.selectors.categoricalhtml.md#skrub.selectors.categorical): Select columns with categorical data types
- [`string()`](../../reference/generated/skrub.selectors.stringhtml.md#skrub.selectors.string): Select columns with string data types
- [`object()`](../../reference/generated/skrub.selectors.objecthtml.md#skrub.selectors.object): Select columns with the `object` (pandas) or `pl.Object` (polars) dtype
- [`boolean()`](../../reference/generated/skrub.selectors.booleanhtml.md#skrub.selectors.boolean): Select columns with boolean data types

## Selectors based on column content and properties

- [`cardinality_below()`](../../reference/generated/skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below): Select columns with fewer unique
  values than a threshold
- [`has_nulls()`](../../reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls): Select columns that contain at least one
  null value

## Selectors based on column names

- [`cols()`](../../reference/generated/skrub.selectors.colshtml.md#skrub.selectors.cols): Select columns explicitly by name
- [`glob()`](../../reference/generated/skrub.selectors.globhtml.md#skrub.selectors.glob): Select columns by name using Unix shell-style
  pattern matching
- [`regex()`](../../reference/generated/skrub.selectors.regexhtml.md#skrub.selectors.regex): Select columns by name using regular expressions
