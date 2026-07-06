# any_date

### skrub.selectors.any_date()

Select columns that have a Date or Datetime data type.

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
str_                 ...
dtype: object
```

```pycon
>>> s.select(df, s.any_date())
                   dt                      tzdt
0 2020-03-02 10:30:00 2020-03-02 10:30:00+00:00
```

<!-- !! processed by numpydoc !! -->
