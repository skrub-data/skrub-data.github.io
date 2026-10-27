# make_retail_events

### skrub.datasets.make_retail_events(n_users=200, n_events=5000, random_state=None)

Generate a synthetic e-commerce clickstream dataset for classification.

Each row represents one user interaction event on a retail platform.
The dataset is designed to showcase [`SessionEncoder`](skrub.SessionEncoderhtml.md#skrub.SessionEncoder) (which
groups events into sessions using `user_id` and `timestamp`),
[`DatetimeEncoder`](skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) (which extracts hour-of-day, day-of-week,
etc. from `timestamp`), one-hot encoding of categorical features, and
scaling of numerical features.

The binary target `converted` indicates whether a purchase occurred
during the session that contains this event.  All events belonging to the
same session share the same `converted` value (a session either converts
or it does not).  The probability of conversion is determined at the
session level by the most intent-rich event type in the session
(`add_to_cart` > `wishlist` > `search` > `page_view`), the
dominant device, and the mean price viewed — so the signal is learnable
directly from the observable features.

* **Parameters:**
  **n_users**
  : Number of distinct users in the dataset.

  **n_events**
  : Approximate total number of events (rows) to generate.  The actual
    count may differ slightly because session sizes are drawn from a
    Poisson distribution.

  **random_state**
  : Controls the random number generation for reproducibility.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following attributes:
    - `X` : [`DataFrame`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html#pandas.DataFrame) with columns:
      - `user_id` : str — user identifier.
      - `timestamp` : [`Timestamp`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Timestamp.html#pandas.Timestamp) — event time.
      - `device_type` : str — one of `"mobile"`, `"desktop"`,
        `"tablet"`.
      - `page_category` : str — one of `"electronics"`, `"fashion"`,
        `"home"`, `"sports"`, `"books"`.
      - `event_type` : str — one of `"page_view"`, `"search"`,
        `"add_to_cart"`, `"wishlist"`.
      - `time_on_page` : float — seconds spent on the page (exponential
        distribution, mean ~ 120 s).
      - `price_viewed` : float — price of the item viewed (log-normal).
    - `y` : [`Series`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html#pandas.Series) of bool, name `"converted"` — the
      classification target.

### Examples

```pycon
>>> from skrub.datasets import make_retail_events
>>> bunch = make_retail_events(n_users=20, n_events=100, random_state=0)
>>> bunch.X.shape[1]  # 7 feature columns; rows ~ n_events
7
>>> bunch.X.columns.tolist()
['user_id', 'timestamp', 'device_type', 'page_category', 'event_type', 'time_on_page', 'price_viewed']
>>> bunch.y.name
'converted'
>>> bunch.y.dtype
dtype('bool')
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>
