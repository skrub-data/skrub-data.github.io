<a id="user-guide-single-column-transformer"></a>

# Advanced columnwise operations

<a id="single-column-transformer"></a>

## The single column transformer

In cases where we want to apply a custom transformation to a series we need the [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols)
structure to handle multiple columns, and if this transformation needs to be able to reject certain
columns and communicate this to [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols), we must to create a transformer from scratch
that raises this exception when appropriate: this can be done with the `core.SingleColumnTranformer` class.

For instance, we might want to create a custom transformer specialized in parsing zip codes:
in this example, the zip codes need to have the format `AB123`, that is two letters
followed by three digits.

```pycon
>>> import pandas as pd
>>> df = pd.DataFrame({'sent': ["AB123", "BD601", "HS014"], 'received': ["AB1C45", "DU3K93", "WB9M88"]})
>>> df
    sent received
0  AB123   AB1C45
1  BD601   DU3K93
2  HS014   WB9M88
```

We would like to be able to “unpack” the zip code so that we have a column for the
letters and one for the digits; the transformer should also be able to “reject” a column
if it does not satisfy the format we specify. A “rejected” column should be passed
through unchanged, as it cannot be handled by this particular transformer.

We can therefore define a custom class that inherits from `core.SingleColumnTranformer`
and that raises [`core.RejectColumn`](../../reference/generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) if a column cannot be handled:

```pycon
>>> from skrub.core import RejectColumn, SingleColumnTransformer
>>> class ZipcodeParser(SingleColumnTransformer):
...     def __init__(self):
...         return
...     def fit_transform(self, X, y=None):
...         if any(X.map(len) != 5):
...             raise RejectColumn('This transformer only takes zip codes of length 5.')
...         else:
...             letters = X.map(lambda s: s[:2])
...             try:
...                 numbers = X.map(lambda s: int(s[2:]))
...             except:
...                 raise RejectColumn('Input zip codes must consist of two letters followed by three numbers.')
...             return(pd.DataFrame({'letters': letters, 'numbers': numbers}))
>>> ZipcodeParser().fit_transform(df["sent"])
  letters  numbers
0      AB      123
1      BD      601
2      HS       14
```

We can use [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) to apply this transformer to the entire dataframe at once,
and set `allow_reject=True` to let rejected columns through without changes:

```pycon
>>> from skrub import ApplyToCols
>>> ApplyToCols(ZipcodeParser(), allow_reject=True).fit_transform(df)
letters  numbers received
0      AB      123   AB1C45
1      BD      601   DU3K93
2      HS       14   WB9M88
```

Note how the `"received"` column has been “rejected” and passed through unmodified.

## Rejection handling with [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) and [`core.RejectColumn`](../../reference/generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn)

The combination [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) and [`core.RejectColumn`](../../reference/generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) allows allows flexible manipulation
and error checking of dataframe. In the previous example, we decided to ignore the
malformed `"received"` column by setting `allow_reject=True`. If, however,
we want our transformer to fail if it encounters a column that it cannot parse,
we can keep the default value of `allow_reject=False`, so that the transform
fails as soon as a malformed column is encountered:

```pycon
>>> ApplyToCols(ZipcodeParser()).fit_transform(df)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: This transformer only takes zip codes of length 5.
Transformer ZipcodeParser.fit_transform failed on column 'received'. See above for the full traceback.
Letting rejected columns through can be useful for situations in which we do not
know the content of a column in advance, like when we are trying to convert to
datetime columns in a dataframe, without knowing which ones actually contain dates.
```

```pycon
>>> from skrub import ToDatetime
>>> df = pd.DataFrame(dict(birthday=["29/01/2024"], city=["London"]))
>>> df
        birthday    city
0  29/01/2024  London
>>> df.dtypes
birthday    ...
city        ...
dtype: object
```

Converting a datetime column would work:

```pycon
>>> ToDatetime().fit_transform(df["birthday"])
0   2024-01-29
Name: birthday, dtype: datetime64[...]
```

While non-datetimes would raise [`core.RejectColumn`](../../reference/generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn):

```pycon
>>> ToDatetime().fit_transform(df["city"])
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Could not find a datetime format for column 'city'.
```

The `allow_reject` parameter in [`ApplyToCols`](../../reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) allows to apply the same transformer
to all columns without having to worry about which columns will actually be converted:
here, [`ToDatetime`](../../reference/generated/skrub.ToDatetimehtml.md#skrub.ToDatetime) is applied only to the “birthday” column, while “city” is passed
through unchanged and no exception is raised.

```pycon
>>> to_datetime = ApplyToCols(ToDatetime(), allow_reject=True)
>>> transformed = to_datetime.fit_transform(df)
>>> transformed
    birthday    city
0 2024-01-29  London
```

We can see that the only column that has a transformer is “birthday”:

```pycon
>>> to_datetime.transformers_
{'birthday': ToDatetime()}
```
