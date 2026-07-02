# ToDatetime

### *class* skrub.ToDatetime(format=None)

Parse datetimes represented as strings and return `Datetime` columns.

This transformer tries to convert the given column from string to datetime,
by either testing common datetime formats or using the `format` specified
by the user. Columns that are not strings, dates or datetimes raise an exception.

* **Parameters:**
  **format**
  : Format to use for parsing dates that are stored as strings, e.g.
    `"%Y-%m-%dT%H:%M%S"`.
    If not specified, the format is inferred from the data when possible.
    When doing so, for dates presented as 01/02/2003, it is usually
    possible to infer from the data whether the month comes first (USA
    convention) or the day comes first, ie `"%m/%d/%Y"` vs
    `"%d/%m/%Y"`. In the odd chance that all the sampled dates land
    before the 13th day of the month and that both conventions are
    plausible, the USA convention (month first) is chosen.
* **Attributes:**
  **format_**
  : Detected format. If the transformer was fitted on a column that already had
    a Datetime dtype, the `format_` is None. Otherwise it is the
    format that was detected when parsing the string column. If the parameter
    `format` was provided, it is the only one that the transformer
    attempts to use so in that caset `format_` is either `None` or
    equal to `format`.

  **output_dtype_**
  : The output dtype, which includes information about the time resolution and
    time zone.

  **output_time_zone_**
  : The time zone of the transformed column. If the output is time zone naive it
    is `None`; otherwise it is the name of the time zone such as `UTC` or
    `Europe/Paris`.

### Notes

An input column is converted to a column with dtype Datetime if possible,
and rejected by raising a `RejectColumn` exception otherwise. Only Date,
Datetime, String, and pandas object columns are handled, other dtypes are
rejected with `RejectColumn`.

Once a column is accepted, outputs of `transform` always have the same
Datetime dtype (including resolution and time zone). Once the transformer
is fitted, entries that fail to be converted during subsequent calls to
`transform` are replaced with nulls.

### Examples

```pycon
>>> import pandas as pd
```

```pycon
>>> s = pd.Series(["2024-05-05T13:17:52", None, "2024-05-07T13:17:52"], name="when")
>>> s
0    2024-05-05T13:17:52
1                    ...
2    2024-05-07T13:17:52
Name: when, dtype: ...
```

```pycon
>>> from skrub import ToDatetime
```

```pycon
>>> to_dt = ToDatetime()
>>> to_dt.fit_transform(s)
0   2024-05-05 13:17:52
1                   NaT
2   2024-05-07 13:17:52
Name: when, dtype: datetime64[...]
```

The attributes `format_`, `output_dtype_`, `output_time_zone_`
record information about the conversion result.

```pycon
>>> to_dt.format_
'%Y-%m-%dT%H:%M:%S'
>>> to_dt.output_dtype_
dtype('<M8[...]')
>>> to_dt.output_time_zone_ is None
True
```

If we provide the datetime format, it is used and columns that do not conform to
it are rejected.

```pycon
>>> ToDatetime(format="%Y-%m-%dT%H:%M:%S").fit_transform(s)
0   2024-05-05 13:17:52
1                   NaT
2   2024-05-07 13:17:52
Name: when, dtype: datetime64[...]
```

```pycon
>>> ToDatetime(format="%d/%m/%Y").fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Failed to convert column 'when' to datetimes using the format '%d/%m/%Y'.
```

Columns that already have `Datetime` `dtype` are not modified (but
they are accepted); for those columns the provided format, if any, is ignored.

```pycon
>>> s = pd.to_datetime(s).dt.tz_localize("Europe/Paris")
>>> s
0   2024-05-05 13:17:52+02:00
1                         NaT
2   2024-05-07 13:17:52+02:00
Name: when, dtype: datetime64[..., Europe/Paris]
>>> to_dt.fit_transform(s) is s
True
```

In that case the `format_` is `None`.

```pycon
>>> to_dt.format_ is None
True
>>> to_dt.output_dtype_
datetime64[..., Europe/Paris]
>>> to_dt.output_time_zone_
'Europe/Paris'
```

Columns that have a different `dtype` than strings, pandas objects, or
datetimes are rejected.

```pycon
>>> s = pd.Series([2020, 2021, 2022], name="year")
>>> to_dt.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'year' does not contain strings.
```

String columns that do not appear to contain datetimes or for some other reason
fail to be converted are also rejected.

```pycon
>>> s = pd.Series(["2024-05-07T13:36:27", "yesterday"], name="when")
>>> to_dt.fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Could not find a datetime format for column 'when'.
```

Once `ToDatetime` was successfully fitted, `transform` will always try to
parse datetimes with the same format and output the same `dtype`. Entries that
fail to be converted result in a null value:

```pycon
>>> s = pd.Series(["2024-05-05T13:17:52", None, "2024-05-07T13:17:52"], name="when")
>>> to_dt = ToDatetime().fit(s)
>>> to_dt.transform(s)
0   2024-05-05 13:17:52
1                   NaT
2   2024-05-07 13:17:52
Name: when, dtype: datetime64[...]
>>> s = pd.Series(["05/05/2024", None, "07/05/2024"], name="when")
>>> to_dt.transform(s)
0   NaT
1   NaT
2   NaT
Name: when, dtype: datetime64[...]
```

**Time zones**

During `fit`, parsing strings that contain fixed offsets results in datetimes
in UTC. Mixed offsets are supported and will all be converted to UTC.

```pycon
>>> s = pd.Series(["2020-01-01T04:00:00+02:00", "2020-01-01T04:00:00+03:00"])
>>> to_dt.fit_transform(s)
0   2020-01-01 02:00:00+00:00
1   2020-01-01 01:00:00+00:00
dtype: datetime64[..., UTC]
>>> to_dt.format_
'%Y-%m-%dT%H:%M:%S%z'
>>> to_dt.output_time_zone_
'UTC'
```

Strings with no timezone indication result in naive datetimes:

```pycon
>>> s = pd.Series(["2020-01-01T04:00:00", "2020-01-01T04:00:00"])
>>> to_dt.fit_transform(s)
0   2020-01-01 04:00:00
1   2020-01-01 04:00:00
dtype: datetime64[...]
>>> to_dt.output_time_zone_ is None
True
```

During `transform`, outputs are cast to the same `dtype` that was found
during `fit`. This includes the timezone, which is converted if necessary.

```pycon
>>> s_paris = pd.to_datetime(
...     pd.Series(["2024-05-07T14:24:49", "2024-05-06T14:24:49"])
... ).dt.tz_localize("Europe/Paris")
>>> s_paris
0   2024-05-07 14:24:49+02:00
1   2024-05-06 14:24:49+02:00
dtype: datetime64[..., Europe/Paris]
>>> to_dt = ToDatetime().fit(s_paris)
>>> to_dt.output_dtype_
datetime64[..., Europe/Paris]
```

Here our converter is set to output datetimes with nanosecond resolution,
localized in “Europe/Paris”.

We may have a column in a different timezone:

```pycon
>>> s_london = s_paris.dt.tz_convert("Europe/London")
>>> s_london
0   2024-05-07 13:24:49+01:00
1   2024-05-06 13:24:49+01:00
dtype: datetime64[..., Europe/London]
```

Here the timezone is “Europe/London” and the times are offset by 1 hour. During
`transform` datetimes will be converted to the original dtype and the
“Europe/Paris” timezone:

```pycon
>>> to_dt.transform(s_london)
0   2024-05-07 14:24:49+02:00
1   2024-05-06 14:24:49+02:00
dtype: datetime64[..., Europe/Paris]
```

Moreover, we may have to transform a timezone-naive column whereas the
transformer was fitted on a timezone-aware column. Note that is somewhat a
corner case unlikely to happen in practice if the inputs to `fit` and
`transform` come from the same dataframe.

```pycon
>>> s_naive = s_paris.dt.tz_convert(None)
>>> s_naive
0   2024-05-07 12:24:49
1   2024-05-06 12:24:49
dtype: datetime64[...]
```

In this case, we make the arbitrary choice to assume that the timezone-naive
datetimes are in UTC.

```pycon
>>> to_dt.transform(s_naive)
0   2024-05-07 14:24:49+02:00
1   2024-05-06 14:24:49+02:00
dtype: datetime64[..., Europe/Paris]
```

Conversely, a transformer fitted on a timezone-naive column can convert
timezone-aware columns. Here also, we assume the naive datetimes were in UTC.

```pycon
>>> to_dt = ToDatetime().fit(s_naive)
>>> to_dt.transform(s_london)
0   2024-05-07 12:24:49
1   2024-05-06 12:24:49
dtype: datetime64[...]
```

**\`\`%d/%m/%Y\`\` vs \`\`%m/%d/%Y\`\`**

When parsing strings in one of the formats above, `ToDatetime` tries to guess
if the month comes first (USA convention) or the day (rest of the world) from
the data.

```pycon
>>> s = pd.Series(["05/23/2024"])
>>> to_dt.fit_transform(s)
0   2024-05-23
dtype: datetime64[...]
>>> to_dt.format_
'%m/%d/%Y'
```

Here we could infer `'%m/%d/%Y'` because there are not 23 months in a year.
Similarly,

```pycon
>>> s = pd.Series(["23/05/2024"])
>>> to_dt.fit_transform(s)
0   2024-05-23
dtype: datetime64[...]
>>> to_dt.format_
'%d/%m/%Y'
```

In the case it cannot be inferred, the USA convention is used:

```pycon
>>> s = pd.Series(["03/05/2024"])
>>> to_dt.fit_transform(s)
0   2024-03-05
dtype: datetime64[...]
>>> to_dt.format_
'%m/%d/%Y'
```

If the days are randomly distributed and the fitting data large enough, it is
somewhat unlikely that all days would be below 12 so the inferred format should
often be correct. To be sure, one can specify the `format` in the
constructor.

### Methods

| [`fit`](#skrub.ToDatetime.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|--------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.ToDatetime.fit_transform)(column[, y])                      | Fit the encoder and transform a column.                                                |
| [`get_feature_names_out`](#skrub.ToDatetime.get_feature_names_out)([input_features]) | Get the output feature names.                                                          |
| [`get_params`](#skrub.ToDatetime.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.ToDatetime.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.ToDatetime.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.ToDatetime.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.ToDatetime.transform)(column)                                   | Transform a column.                                                                    |
<!-- !! processed by numpydoc !! -->

#### fit(column, y=None, \*\*kwargs)

Fit the transformer.

This default implementation simply calls `fit_transform()` and
returns `self`.

Subclasses should implement `fit_transform` and `transform`.

* **Parameters:**
  **column**
  : Unlike most scikit-learn transformers, single-column transformers
    transform a single column, not a whole dataframe.

  **y**
  : Prediction targets.

  **\*\*kwargs**
  : Extra named arguments are passed to `self.fit_transform()`.
* **Returns:**
  self
  : The fitted transformer.

<!-- !! processed by numpydoc !! -->

#### fit_transform(column, y=None)

Fit the encoder and transform a column.

* **Parameters:**
  **column**
  : The input to transform.

  **y**
  : Ignored.
* **Returns:**
  **transformed**
  : The input transformed to Datetime.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Get the output feature names.

* **Parameters:**
  **input_features**
  : Input feature names. Ignored.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The names of the output features.

<!-- !! processed by numpydoc !! -->

#### get_params(deep=True)

Get parameters for this estimator.

* **Parameters:**
  **deep**
  : If True, will return the parameters for this estimator and
    contained subobjects that are estimators.
* **Returns:**
  **params**
  : Parameter names mapped to their values.

<!-- !! processed by numpydoc !! -->

#### set_output(, transform=None)

Default no-op implementation for set_output.

Skrub transformers already output dataframes of the correct type by
default so there is usually no need for set_output to do anything.

Subclasses are of course free to redefine set_output (e.g. by
inheriting from `TransformerMixin` before SingleColumnTransformer).

* **Parameters:**
  **transform**
  : Ignored.
* **Returns:**
  SingleColumnTransformer
  : Returns self.

<!-- !! processed by numpydoc !! -->

#### set_params(\*\*params)

Set the parameters of this estimator.

The method works on simple estimators as well as on nested objects
(such as [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)). The latter have
parameters of the form `<component>__<parameter>` so that it’s
possible to update each component of a nested object.

* **Parameters:**
  **\*\*params**
  : Estimator parameters.
* **Returns:**
  **self**
  : Estimator instance.

<!-- !! processed by numpydoc !! -->

#### set_transform_request(, column='$UNCHANGED$')

Configure whether metadata should be requested to be passed to the `transform` method.

Note that this method is only relevant when this estimator is used as a
sub-estimator within a [meta-estimator](https://scikit-learn.org/stable/glossary.html#term-meta-estimator) and metadata routing is enabled
with `enable_metadata_routing=True` (see [`sklearn.set_config()`](https://scikit-learn.org/stable/modules/generated/sklearn.set_config.html#sklearn.set_config)).
Please check the [User Guide](https://scikit-learn.org/stable/metadata_routing.html#metadata-routing) on how the routing
mechanism works.

The options for each parameter are:

- `True`: metadata is requested, and passed to `transform` if provided. The request is ignored if metadata is not provided.
- `False`: metadata is not requested and the meta-estimator will not pass it to `transform`.
- `None`: metadata is not requested, and the meta-estimator will raise an error if the user provides it.
- `str`: metadata should be passed to the meta-estimator with this given alias instead of the original name.

The default (`sklearn.utils.metadata_routing.UNCHANGED`) retains the
existing request. This allows you to change the request for some
parameters and not others.

#### Versionadded
Added in version 1.3.

* **Parameters:**
  **column**
  : Metadata routing for `column` parameter in `transform`.
* **Returns:**
  **self**
  : The updated object.

<!-- !! processed by numpydoc !! -->

#### transform(column)

Transform a column.

* **Parameters:**
  **column**
  : The input to transform.
* **Returns:**
  **transformed**
  : The input transformed to Datetime.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we illustrate how to better integrate datetime features in machine learning models with the |DatetimeEncoder|.">  <div class="sphx-glr-thumbnail-title">Handling datetime features with the DatetimeEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>
