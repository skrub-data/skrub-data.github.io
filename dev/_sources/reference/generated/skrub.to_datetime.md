# to_datetime

### skrub.to_datetime(data, format=None)

Convert a dataframe or series to Datetime dtype.

This function tries to convert the given dataframe or series from string to datetime,
by either testing common datetime formats or using the `format` specified
by the user. Columns that cannot be parsed are returned unchanged.

Note that this transformation is stateless, so it should not be used in a
pipeline that is fitted on a training set and then applied to a test set.
Use the [`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime) transformer instead.

* **Parameters:**
  **data**
  : The dataframe or series to convert to Datetime.

  **format**
  : Format string to use to parse datetime strings.
    See the reference documentation for format codes:
    [https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes) .
* **Returns:**
  **output**
  : The input transformed to Datetime.

#### SEE ALSO
[`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime)
: Parse datetimes represented as strings and return Datetime columns.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import to_datetime
>>> X = pd.DataFrame(dict(a=[1, 2], b=["01/02/2021", "21/02/2021"]))
>>> X
   a           b
0  1  01/02/2021
1  2  21/02/2021
>>> to_datetime(X)
   a          b
0  1 2021-02-01
1  2 2021-02-21
```

<!-- !! processed by numpydoc !! -->
