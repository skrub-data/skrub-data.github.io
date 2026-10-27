# any_date

### skrub.selectors.any_date()

Select columns that have a Date or Datetime data type.

#### SEE ALSO
[`skrub.Cleaner`](skrub.Cleanerhtml.md#skrub.Cleaner)
: Parse and clean date columns into proper datetime types.

[`skrub.ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime)
: Convert string columns to datetime types.

[`skrub.DatetimeEncoder`](skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder)
: Encode datetime columns into numeric features for machine learning.

### Notes

Only datetime columns are selected. Time-only, period, and duration types are
not selected.
Selection is based on the column’s dtype: for example string columns containing
date-like values are not selected.

Selected columns depend on the dataframe library and its supported dtypes:
in pandas, this selector selects columns with dtype `datetime64[ns]`,
while in polars, it selects both `Date` and `Datetime` dtypes.

### Examples

```pycon
>>> import datetime
>>> from skrub import selectors as s
>>> import pandas as pd
```

```pycon
>>> df = pd.DataFrame(
...     dict(
...         dt=[datetime.datetime(2020, 3, 2, 10, 30)],
...         tzdt=[
...             datetime.datetime(2020, 3, 2, 10, 30, tzinfo=datetime.timezone.utc)
...         ],
...         str_=["2020-03-02 10:30:00"],
...     )
... )
>>> df
                   dt                      tzdt                 str_
0 2020-03-02 10:30:00 2020-03-02 10:30:00+00:00  2020-03-02 10:30:00
```

```pycon
>>> df.dtypes
dt           datetime64[...]
tzdt    datetime64[..., UTC]
str_                     ...
dtype: object
```

Select all date/datetime columns:

```pycon
>>> s.select(df, s.any_date())
                       dt                      tzdt
0 2020-03-02 10:30:00 2020-03-02 10:30:00+00:00
```

Note that string columns with date-like values are not selected
(use filtering for that):

```pycon
>>> s.select(df, s.any_date() | s.string())
                       dt                      tzdt                 str_
0 2020-03-02 10:30:00 2020-03-02 10:30:00+00:00  2020-03-02 10:30:00
```

<!-- !! processed by numpydoc !! -->
