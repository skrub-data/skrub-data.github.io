# DatetimeEncoder

### *class* skrub.DatetimeEncoder(resolution='hour', add_weekday=False, add_total_seconds=True, add_day_of_year=False, periodic_encoding=None)

Extract temporal features such as month, day of the week, … from a datetime column.

The `DatetimeEncoder` converts datetime features to numerical features that
can be used by learners. It separates each datetime in its parts (year, month,
day, etc.), and can add new features based on the datetime (weekday, seconds
from epoch, day of year). Circular or spline-based periodic features may also
be included.

* **Parameters:**
  **resolution**
  : If a string, extract up to this resolution. Must be “year”, “month”,
    “day”, “hour”, “minute”, “second”, “microsecond”, or “nanosecond”. For
    example, `resolution="day"` generates the features “year”, “month”,
    and “day” only. If the input column contains dates with no time
    information, time features (“hour”, “minute”, … ) are never extracted.
    If `None`, the features listed above are not extracted (but day of
    the week and total seconds may still be extracted, see below).

  **add_weekday**
  : Extract the day of the week as a numerical feature from 1 (Monday) to 7
    (Sunday).

  **add_total_seconds**
  : Add the total number of seconds since the Unix epoch (00:00:00 UTC on 1
    January 1970).

  **add_day_of_year**
  : Add the day of year (ordinal day) as an integer in the range 1 to 365 (or
    366 in the case of leap years). January 1st will be day 1, December 31st
    will be day 365 on non-leap years.

  **periodic_encoding**
  : Add periodic features with different granularities. Add periodic features
    using either trigonometric (`circular`) or `spline` encoding.
    If `None`, no periodic encoding is applied.
* **Attributes:**
  **extracted_features_**
  : The features that are extracted, a subset of [“year”, …, “nanosecond”,
    “weekday”, “total_seconds”, “day_of_year”]. If `periodic_encoding` is set to
    either `circular` or `spline`, the extracted periodic features will also be
    added. Given a feature named `date`, new features will be named
    `date_month_circular_0`, `date_month_circular_1` etc., or
    `date_month_spline_00`, `date_month_spline_01` etc., accordingly.

#### SEE ALSO
[`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime)
: Convert strings to datetimes.

### Notes

All extracted features are provided as float32 columns.

No timezone conversion is performed: if the input column is timezone aware, the
extracted features will be in the column’s timezone.

An input column that does not have a Date or Datetime dtype will be
rejected by raising a [`RejectColumn`](skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) exception. See `ToDatetime` for
converting strings to proper datetimes.
**Note:** the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) only sends datetime columns to its
`datetime_encoder`. Therefore it is always safe to use a `DatetimeEncoder` as
the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer)’s `datetime_encoder` parameter.

The `DatetimeEncoder` uses hardcoded values for generating periodic features.
The period of each feature is:

- `month`: 12 (month in year)
- `day`: 30 (day in month)
- `hour`: 24 (hour in day)
- `weekday`: 7 (day in week)

Additionally, we specify the number of splines for each feature to avoid
generating too many features:

- `month`: 12
- `day`: 4
- `hour`: 12
- `weekday`: 7

### Examples

```pycon
>>> import pandas as pd
>>> login = pd.to_datetime(
...     pd.Series(
...         ["2024-05-13T12:05:36", None, "2024-05-15T13:46:02"], name="login")
... )
>>> login
0   2024-05-13 12:05:36
1                   NaT
2   2024-05-15 13:46:02
Name: login, dtype: datetime64[...]
>>> from skrub import DatetimeEncoder
>>> DatetimeEncoder().fit_transform(login)
   login_year  login_month  login_day  login_hour  login_total_seconds
0      2024.0          5.0       13.0        12.0         1.715602e+09
1         NaN          NaN        NaN         NaN                  NaN
2      2024.0          5.0       15.0        13.0         1.715781e+09
```

We can ask for a finer resolution:

```pycon
>>> DatetimeEncoder(resolution='second', add_total_seconds=False).fit_transform(
...     login
... )
   login_year  login_month  login_day  login_hour  login_minute  login_second
0      2024.0          5.0       13.0        12.0           5.0          36.0
1         NaN          NaN        NaN         NaN           NaN           NaN
2      2024.0          5.0       15.0        13.0          46.0           2.0
```

We can also ask for the day of the week. The week starts at 1 on Monday and ends
at 7 on Sunday. This is consistent with the
[ISO week date system](https://en.wikipedia.org/wiki/ISO_week_date),
the standard library
[`datetime.isoweekday()`](https://docs.python.org/3/library/datetime.html#datetime.datetime.isoweekday) and polars
[`weekday`](https://docs.pola.rs/py-polars/html/reference/series/api/polars.Series.dt.weekday.html#polars.Series.dt.weekday), but not with pandas
[`day_of_week`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.day_of_week.html#pandas.Series.dt.day_of_week), which counts days from 0.

```pycon
>>> login.dt.strftime('%A = %w')
0       Monday = 1
1              NaN
2    Wednesday = 3
Name: login, dtype: ...
>>> login.dt.day_of_week
0    0.0
1    NaN
2    2.0
Name: login, dtype: float64
>>> DatetimeEncoder(add_weekday=True, add_total_seconds=False).fit_transform(login)
   login_year  login_month  login_day  login_hour  login_weekday
0      2024.0          5.0       13.0        12.0            1.0
1         NaN          NaN        NaN         NaN            NaN
2      2024.0          5.0       15.0        13.0            3.0
```

When a column contains only dates without time information, the time features
are discarded, regardless of `resolution`.

```pycon
>>> birthday = pd.to_datetime(
...     pd.Series(['2024-04-14', '2024-05-15'], name='birthday')
... )
>>> encoder = DatetimeEncoder(resolution='second')
>>> encoder.fit_transform(birthday)
   birthday_year  birthday_month  birthday_day  birthday_total_seconds
0         2024.0             4.0          14.0            1.713053e+09
1         2024.0             5.0          15.0            1.715731e+09
>>> encoder.extracted_features_
['year', 'month', 'day', 'total_seconds']
>>> encoder.all_outputs_
['birthday_year', 'birthday_month', 'birthday_day', 'birthday_total_seconds']
```

(The number of seconds since Epoch can still be extracted but not “hour”,
“minute”, etc.)

Non-datetime columns are rejected by raising a [`RejectColumn`](skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn)
exception.

```pycon
>>> s = pd.Series(['2024-04-14', '2024-05-15'], name='birthday')
>>> s
0    2024-04-14
1    2024-05-15
Name: birthday, dtype: ...
>>> DatetimeEncoder().fit_transform(s)
Traceback (most recent call last):
    ...
skrub.core.RejectColumn: Column 'birthday' does not have Date or Datetime dtype.
```

[`ToDatetime`](skrub.ToDatetimehtml.md#skrub.ToDatetime): can be used for converting strings to datetimes.

```pycon
>>> from skrub import ToDatetime
>>> from sklearn.pipeline import make_pipeline
>>> make_pipeline(ToDatetime(), DatetimeEncoder()).fit_transform(s)
   birthday_year  birthday_month  birthday_day  birthday_total_seconds
0         2024.0             4.0          14.0            1.713053e+09
1         2024.0             5.0          15.0            1.715731e+09
```

**Time zones**

If the input column has a time zone, the extracted features are in this time zone.

```pycon
>>> login = pd.to_datetime(
...     pd.Series(
...         ["2024-05-13T12:05:36", None, "2024-05-15T13:46:02"], name="login")
... ).dt.tz_localize('Europe/Paris')
>>> encoder = DatetimeEncoder()
>>> encoder.fit_transform(login)['login_hour']
0    12.0
1     NaN
2    13.0
Name: login_hour, dtype: float32
```

No special care is taken to convert inputs to `transform` to the same time
zone as the column the encoder was fitted on. The features are always in the
time zone of the input.

```pycon
>>> login_sp = login.dt.tz_convert('America/Sao_Paulo')
>>> login_sp
0   2024-05-13 07:05:36-03:00
1                         NaT
2   2024-05-15 08:46:02-03:00
Name: login, dtype: datetime64[..., America/Sao_Paulo]
>>> encoder.transform(login_sp)['login_hour']
0    7.0
1    NaN
2    8.0
Name: login_hour, dtype: float32
```

To ensure datetime columns are in a consistent timezones, use `ToDatetime`.

```pycon
>>> encoder = make_pipeline(ToDatetime(), DatetimeEncoder())
>>> encoder.fit_transform(login)['login_hour']
0    12.0
1     NaN
2    13.0
Name: login_hour, dtype: float32
>>> encoder.transform(login_sp)['login_hour']
0    12.0
1     NaN
2    13.0
Name: login_hour, dtype: float32
```

Here we can see the Sao Paulo input of the encoder has been converted back to the
time zone used during the fitting and that we get the same result for “hour”.

The DatetimeEncoder can also create new features based on either trigonometric
functions or splines by setting `periodic_encoder="circular"` or
`periodic_encoder="spline"` respectively.
See [this example](https://scikit-learn.org/stable/auto_examples/applications/plot_cyclical_feature_engineering.html)
in scikit-learn to know more about cyclical feature engineering.

```pycon
>>> encoder = make_pipeline(ToDatetime(), DatetimeEncoder(periodic_encoding="circular"))
>>> encoder.fit_transform(login)
   login_year  ...  login_hour_circular_1
0      2024.0  ...              -1.000000
1         NaN  ...                    NaN
2      2024.0  ...              -0.965926
```

Added features can be explored using `DatetimeEncoder.all_outputs_`:

```pycon
>>> encoder[-1].all_outputs_
['login_year', 'login_total_seconds', 'login_month_circular_0', 'login_month_circular_1',
    'login_day_circular_0', 'login_day_circular_1', 'login_hour_circular_0', 'login_hour_circular_1']
```

### Methods

| [`fit`](#skrub.DatetimeEncoder.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.DatetimeEncoder.fit_transform)(column[, y])                      | Fit the encoder and transform a column.                                                |
| [`get_feature_names_out`](#skrub.DatetimeEncoder.get_feature_names_out)([input_features]) | Get output feature names for transformation.                                           |
| [`get_params`](#skrub.DatetimeEncoder.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.DatetimeEncoder.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.DatetimeEncoder.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.DatetimeEncoder.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.DatetimeEncoder.transform)(column)                                   | Transform a column.                                                                    |
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
  : The extracted features.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Get output feature names for transformation.

* **Parameters:**
  **input_features**
  : Ignored.
* **Returns:**
  **feature_names_out**
  : Transformed feature names.

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
  : The extracted features.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="The following example illustrates the use of the SquashingScaler, a transformer that can rescale and squash numerical features to a range that works well with neural networks and perhaps also other related models. Its basic idea is to rescale the features based on quantile statistics (to be robust to outliers), and then perform a smooth squashing function to limit the outputs to a pre-defined range. This transform has been found to even work well when applied to one-hot encoded features.">  <div class="sphx-glr-thumbnail-title">SquashingScaler: Robust numerical preprocessing for neural networks</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we illustrate how to better integrate datetime features in machine learning models with the |DatetimeEncoder|.">  <div class="sphx-glr-thumbnail-title">Handling datetime features with the DatetimeEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>
