# SessionEncoder

### *class* skrub.SessionEncoder(timestamp_col, split_by=None, session_gap=1800, suffix='session_id')

Add a session ID column to a dataframe based on time gaps and other columns.

A session is defined as a sequence of events  where consecutive events are separated
by at most `session_gap` seconds. Additionally, it is possible to provide a column
or list of columns that can be used to distinguish between sessions, such
as user identifiers (specified by the `split_by` column).
Within each sequence identified by a unique value in the `split_by` column(s),
a new session is started when the time gap between events exceeds `session_gap`
seconds.

The encoder takes care of grouping the data by `split_by` and sorting by
timestamp column before identifying sessions, and sorting it back to the
original order at the end, so the original order of events in the input
dataframe does not matter.

If a null value is present in the timestamp column or any of the `split_by`
columns, the corresponding row will be assigned a session ID of -1, and will
be ignored when computing time intervals.

All unrelated columns are passed through unchanged.

* **Parameters:**
  **timestamp_col**
  : The name of the column that identifies the time of an event. This column
    is used to determine the start and end of a session. `timestamp_col` must
    be a datetime, and an error will be raised otherwise.
    Sessions are defined within each group of events that have the same value
    in the `split_by` column(s) (or all events if `split_by` is None), and
    the result will be correct no matter the order of the rows in the input
    dataframe.

  **split_by**
  : The name of the column (typically the user ID), or list of columns, that
    is used to identify independent event sequence (such as the activity of
    different users).
    A session boundary is created when the value in any of these columns
    changes, or when the time gap between events exceeds `session_gap`.
    This is typically a user identifier column, but it can also be used to define
    sessions by other groupings (e.g. user and device type).
    If not provided, sessions are detected based on the time gap between events,
    and all events are considered to belong to the same user (or group).

  **session_gap**
  : The maximum gap (in seconds) between events in a session. If the gap
    between two events exceeds this value, they are considered to be in
    different sessions. Default is 1800 seconds (30 minutes).

  **suffix**
  : The suffix to be added to the name of the created session id column.
* **Attributes:**
  **all_inputs_**
  : All column names in the input dataframe.

  **all_outputs_: list of str**
  : All column names in the input dataframe plus the new column that identifies
    the session, with name “{timestamp}_{suffix}”.

  **session_id_column_**
  : The name of the session ID column that is added to the dataframe. This is
    generated as “{timestamp_col}_{suffix}”, but if this name already exists in
    the input dataframe, a random suffix is added to avoid overwriting it.

### Examples

Consider this example where we have a dataframe with user events, and we want
to identify sessions based on a 30-minute gap between events for each user.
Users are identified by the value of the column `user_id`.
Note that the order of the rows in the input dataframe does not matter.

Sessions are defined by sorting over the 

```
``
```

split_by\`\`columns (if provided)
and then by the timestamp.

```pycon
>>> import pandas as pd
>>> from datetime import datetime, timedelta
>>> data = {
...     "user_id": [1, 1, 1, 1, 1, 2, 2],
...     "device_id": [
...         "mobile",
...         "mobile",
...         "desktop",
...         "desktop",
...         "mobile",
...         "mobile",
...         "mobile",
...     ],
...     "timestamp": [
...         pd.Timestamp("2024-01-01 10:00:00"),
...         pd.Timestamp("2024-01-01 10:10:00"),  # 10 min later, same session
...         pd.Timestamp("2024-01-01 10:05:00"),  # Different device (sorted),
...                                                 # different session
...     pd.Timestamp("2024-01-01 10:20:00"),  # 15 min later, same session
...                                                 # different session
...     pd.Timestamp("2024-01-01 11:20:00"),  # 60 min later, new session
...     pd.Timestamp("2024-01-01 10:00:00"),  # Different user
...     pd.Timestamp("2024-01-01 10:15:00"),  # 15 min later, same session
... ],
...     "action": [
...         "view",
...         "purchase",
...         "view",
...         "add_to_cart",
...         "checkout",
...         "view",
...         "wishlist",
...     ],
... }
>>> df = pd.DataFrame(data)
>>> df
user_id device_id           timestamp       action
0        1    mobile 2024-01-01 10:00:00         view
1        1    mobile 2024-01-01 10:10:00     purchase
2        1   desktop 2024-01-01 10:05:00         view
3        1   desktop 2024-01-01 10:20:00  add_to_cart
4        1    mobile 2024-01-01 11:20:00     checkout
5        2    mobile 2024-01-01 10:00:00         view
6        2    mobile 2024-01-01 10:15:00     wishlist
```

We use the `SessionEncoder` with default `session_gap` of 30 minutes:

```pycon
>>> from skrub import SessionEncoder
>>> encoder = SessionEncoder(
...     split_by='user_id', timestamp_col='timestamp'
... )
>>> result = encoder.fit_transform(df)
>>> result
user_id device_id           timestamp       action  timestamp_session_id
0        1    mobile 2024-01-01 10:00:00         view                     0
1        1    mobile 2024-01-01 10:10:00     purchase                     0
2        1   desktop 2024-01-01 10:05:00         view                     0
3        1   desktop 2024-01-01 10:20:00  add_to_cart                     0
4        1    mobile 2024-01-01 11:20:00     checkout                     1
5        2    mobile 2024-01-01 10:00:00         view                     2
6        2    mobile 2024-01-01 10:15:00     wishlist                     2
```

In this example, grouping by `user_id` results in three separate sessions:

- User 1 has two sessions (session 0 and session 1) because there is a gap of
  60 minutes between their events at 10:20 and 11:20, which exceeds the 30-minute
  threshold. The first four events of user 1 belong to session 0, while the
  last event belongs to session 1.
- User 2 has one session (session 2) because all their events are within
  30 minutes of the previous one.

You can also identify users by multiple columns. For instance, the same user
on different devices should have separate sessions.

```pycon
>>> encoder_multi = SessionEncoder(
...     split_by=['user_id', 'device_id'],
...     timestamp_col='timestamp',
... )
>>> result_multi = encoder_multi.fit_transform(df)
>>> result_multi
user_id device_id           timestamp       action  timestamp_session_id
0        1    mobile 2024-01-01 10:00:00         view                     1
1        1    mobile 2024-01-01 10:10:00     purchase                     1
2        1   desktop 2024-01-01 10:05:00         view                     0
3        1   desktop 2024-01-01 10:20:00  add_to_cart                     0
4        1    mobile 2024-01-01 11:20:00     checkout                     2
5        2    mobile 2024-01-01 10:00:00         view                     3
6        2    mobile 2024-01-01 10:15:00     wishlist                     3
```

In this example:

- User 1 on “desktop” has session 0.
- User 1 on “mobile” has two sessions, session 1 and session 2, because there
  is a gap of 60 minutes between their events at 10:10 and 11:20, which exceeds
  the 30-minute threshold.
- User 2 on “mobile” has session 3 (different user).

Note that the value of the session IDs are arbitrary and depend on the order
of events in the dataframe. The important thing is that events that belong to
the same session have the same session ID, and events that belong to different
sessions have different session IDs.

You can also use `SessionEncoder` without a user identifier column. In this case,
sessions are separated only by time gaps. This is useful for analyzing a single
timeseries or events that don’t have a user dimension:

```pycon
>>> encoder_no_split = SessionEncoder(
...     split_by=None,
...     timestamp_col='timestamp',
... )
>>> data_no_split = {
...     'timestamp': [
...         pd.Timestamp('2024-01-01 10:00:00'),
...         pd.Timestamp('2024-01-01 10:10:00'),  # 10 min gap
...         pd.Timestamp('2024-01-01 10:15:00'),  # 5 min gap, still in session
...         pd.Timestamp('2024-01-01 11:00:00'),  # 45 min gap, new session
...         pd.Timestamp('2024-01-01 11:10:00'),  # 10 min gap, continue session
...     ],
...     'event_type': ['start', 'action', 'action', 'restart', 'action']
... }
>>> df_no_split = pd.DataFrame(data_no_split)
>>> result_no_split = encoder_no_split.fit_transform(df_no_split)
>>> result_no_split
             timestamp event_type  timestamp_session_id
0 2024-01-01 10:00:00      start                     0
1 2024-01-01 10:10:00     action                     0
2 2024-01-01 10:15:00     action                     0
3 2024-01-01 11:00:00    restart                     1
4 2024-01-01 11:10:00     action                     1
```

In this example:

- Events at 10:00, 10:10, and 10:15 form session 0 (all gaps < 30 min).
- The event at 11:00 starts a new session 1 (45 min gap > 30 min).
- The event at 11:10 continues session 1 (10 min gap < 30 min).

It is possible to change the duration of the session gap by setting the
`session_gap` parameter. For example, we can set it to 5 minutes (300 seconds)
instead of the default 30 minutes, and this will change the session assignments
accordingly:

```pycon
>>> encoder_new_gap = SessionEncoder(
...     split_by=None,
...     timestamp_col='timestamp',
...     session_gap=300
... )
>>> result_new_gap = encoder_new_gap.fit_transform(df_no_split)
>>> result_new_gap
            timestamp event_type  timestamp_session_id
0 2024-01-01 10:00:00      start                     0
1 2024-01-01 10:10:00     action                     1
2 2024-01-01 10:15:00     action                     1
3 2024-01-01 11:00:00    restart                     2
4 2024-01-01 11:10:00     action                     3
```

It is also possible to change the suffix that is added at the end of the session
ID column via the “suffix” parameter. This is useful, for example, if you want
to add sessions based on different groupings or intervals:

```pycon
>>> data_multi = {
...     'user_id': [1, 1, 1, 1, 2, 2],
...     'device_id': ['mobile', 'mobile', 'desktop', 'desktop', 'mobile', 'mobile'],
...     'timestamp': [
...         pd.Timestamp('2024-01-01 10:00:00'),
...         pd.Timestamp('2024-01-01 10:10:00'),  # 10 min later, same session
...         pd.Timestamp('2024-01-01 10:05:00'),  # Different device (sorted),
...                                                 # different session
...         pd.Timestamp('2024-01-01 10:20:00'),  # 15 min later, same session
...         pd.Timestamp('2024-01-01 10:00:00'),  # Different user
...         pd.Timestamp('2024-01-01 10:15:00'),  # 15 min later, same session
...     ],
...     'action': ['view', 'purchase', 'view', 'checkout', 'login', 'view']
... }
>>> df = pd.DataFrame(data_multi)
>>> encoder_user = SessionEncoder("timestamp",
... split_by=["user_id"], suffix="user")
>>> encoder_user.fit_transform(df)
user_id device_id           timestamp    action  timestamp_user
0        1    mobile 2024-01-01 10:00:00      view                  0
1        1    mobile 2024-01-01 10:10:00  purchase                  0
2        1   desktop 2024-01-01 10:05:00      view                  0
3        1   desktop 2024-01-01 10:20:00  checkout                  0
4        2    mobile 2024-01-01 10:00:00     login                  1
5        2    mobile 2024-01-01 10:15:00      view                  1
```

```pycon
>>> encoder_user_device = SessionEncoder("timestamp",
... split_by=["user_id", "device_id"],
... suffix="user_device")
>>> encoder_user_device.fit_transform(df)
user_id device_id           timestamp    action  timestamp_user_device
0        1    mobile 2024-01-01 10:00:00      view                      1
1        1    mobile 2024-01-01 10:10:00  purchase                      1
2        1   desktop 2024-01-01 10:05:00      view                      0
3        1   desktop 2024-01-01 10:20:00  checkout                      0
4        2    mobile 2024-01-01 10:00:00     login                      2
5        2    mobile 2024-01-01 10:15:00      view                      2
```

When the timestamp column or any of the split_by columns contains null values,
those rows will be assigned **session ID -1**, and will be ignored when computing
time intervals. In some versions of pandas,
adding nulls to a pandas column of integers will convert it to float, which can
cause issues with grouping if there are a lot of sessions.

```pycon
>>> data_with_nulls = {
...     'user_id': [1, 1, None, 1],  # None value in split_by column
...     'timestamp': [
...         pd.Timestamp('2024-01-01 10:00:00'),
...         None,  # None value in timestamp column
...         pd.Timestamp('2024-01-01 10:10:00'),
...         pd.Timestamp('2024-01-01 10:20:00'),
...     ],
... }
>>> df_with_nulls = pd.DataFrame(data_with_nulls)
>>> encoder_with_nulls = SessionEncoder(
...     split_by='user_id',
...     timestamp_col='timestamp'
... )
>>> result_with_nulls = encoder_with_nulls.fit_transform(df_with_nulls)
>>> result_with_nulls
user_id           timestamp  timestamp_session_id
0      1.0 2024-01-01 10:00:00                     0
1      1.0                 NaT                    -1
2      NaN 2024-01-01 10:10:00                    -1
3      1.0 2024-01-01 10:20:00                     0
```

In this example:

- Row 0 has valid user_id and timestamp, so it gets session ID 0.
- Row 1 has a null timestamp, so it gets session ID -1.
- Row 2 has a null user_id, so it gets session ID -1.
- Row 3 has valid user_id and timestamp, and since there is less than 30 minutes
  between rows 0 and 3 (ignoring row 1 and 2 due to null values), it gets the
  same session ID as row 0.

### Methods

| [`fit`](#skrub.SessionEncoder.fit)(X[, y])                                               | Fit the transformer to the data.                                           |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| [`fit_transform`](#skrub.SessionEncoder.fit_transform)(X[, y])                           | Fit the transformer to the data and return the transformed dataframe.      |
| [`get_feature_names_out`](#skrub.SessionEncoder.get_feature_names_out)([input_features]) | Return the column names of the output of `transform` as a list of strings. |
| [`get_params`](#skrub.SessionEncoder.get_params)([deep])                                 | Get parameters for this estimator.                                         |
| [`set_output`](#skrub.SessionEncoder.set_output)(\*[, transform])                        | Set output container.                                                      |
| [`set_params`](#skrub.SessionEncoder.set_params)(\*\*params)                             | Set the parameters of this estimator.                                      |
| [`transform`](#skrub.SessionEncoder.transform)(X[, y])                                   | Transform the data by encoding sessions.                                   |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit the transformer to the data.

* **Parameters:**
  **X**
  : The input dataframe.

  **y**
  : Ignored.
* **Returns:**
  **self**
  : The fitted transformer.

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None)

Fit the transformer to the data and return the transformed dataframe.

* **Parameters:**
  **X**
  : The input dataframe.

  **y**
  : Ignored.
* **Returns:**
  pandas.DataFrame or polars.DataFrame
  : The transformed dataframe with session information.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Return the column names of the output of `transform` as a list of strings.

* **Parameters:**
  **input_features**
  : Ignored.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of strings
  : The column names.

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

Set output container.

Refer to the [user guide](https://scikit-learn.org/stable/modules/df_output_transform.html#df-output-transform) for more details
and [Introducing the set_output API](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_set_output.html#sphx-glr-auto-examples-miscellaneous-plot-set-output-py) for an
example on how to use the API.

* **Parameters:**
  **transform**
  : Configure output of `transform` and `fit_transform`.
    - `"default"`: Default output format of a transformer
    - `"pandas"`: DataFrame output
    - `"polars"`: Polars output
    - `None`: Transform configuration is unchanged
    <br/>
    #### Versionadded
    Added in version 1.4: `"polars"` option was added.
* **Returns:**
  **self**
  : Estimator instance.

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

#### transform(X, y=None)

Transform the data by encoding sessions.

* **Parameters:**
  **X**
  : The input dataframe.

  **y**
  : Ignored.
* **Returns:**
  pandas.DataFrame or polars.DataFrame
  : The transformed dataframe with session information.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>
