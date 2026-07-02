# RejectColumn

### *exception* skrub.core.RejectColumn

Used by single-column transformers to indicate they do not apply to a column.

### Examples

A [`SingleColumnTransformer`](skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer) can raise `RejectColumn` exceptions
to indicate it cannot handle a given column.

```pycon
>>> from skrub import ToDatetime
>>> import pandas as pd
>>> df = pd.DataFrame(dict(birthday=["29/01/2024"], city=["London"]))
>>> df
     birthday    city
0  29/01/2024  London
>>> df.dtypes
birthday    ...
city        ...
dtype: object
>>> ToDatetime().fit_transform(df["birthday"])
0   2024-01-29
Name: birthday, dtype: datetime64[...]
>>> ToDatetime().fit_transform(df["city"])
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Could not find a datetime format...
```

`RejectColumn` exceptions can be used to indicate that a column cannot be
handled by the current transformer: this may mean that the data is invalid,
or that the transformer is simply not designed to handle that type of data.
For example, a `ToDatetime` transformer might raise `RejectColumn`
when  it is passed a column that does not contain strings, or that contains
strings but none of them look like dates.

`ApplyToCols` relies on these exceptions to decide whether a column
should be transformed or passed through unchanged.

How these rejections are handled depends on the `allow_reject` parameter.
By default, no special handling is performed and rejections are considered
to be errors:

```pycon
>>> from skrub import ApplyToCols
>>> to_datetime = ApplyToCols(ToDatetime())
>>> to_datetime.fit_transform(df)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Could not find a datetime format for column 'city'.
Transformer ToDatetime.fit_transform failed on column 'city'. See above for the full traceback.
```

However, setting `allow_reject=True` gives the transformer itself some
control over which columns it should be applied to. For example, whether a
string column contains dates is only known once we try to parse them.
Therefore it might be sensible to try to parse all string columns but allow
the transformer to reject those that, upon inspection, do not contain dates.

```pycon
>>> to_datetime = ApplyToCols(ToDatetime(), allow_reject=True)
>>> transformed = to_datetime.fit_transform(df)
>>> transformed
    birthday    city
0 2024-01-29  London
```

Now the column ‘city’ was rejected but this was not treated as an error;
‘city’ was passed through unchanged and only ‘birthday’ was converted to a
datetime column.

```pycon
>>> transformed.dtypes
birthday    datetime64[...]
city                ...
dtype: object
>>> to_datetime.transformers_
{'birthday': ToDatetime()}
```

```pycon
>>> from skrub import ToDatetime
>>> df = pd.DataFrame(dict(a=['2020-02-02'], b=[12.5]))
>>> ToDatetime().fit_transform(df['a'])
0   2020-02-02
Name: a, dtype: datetime64[...]
>>> ToDatetime().fit_transform(df['b'])
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'b' does not contain strings.
```

<!-- !! processed by numpydoc !! -->
