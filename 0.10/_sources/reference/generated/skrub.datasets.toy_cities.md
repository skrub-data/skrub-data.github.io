# toy_cities

### skrub.datasets.toy_cities(seed=0, size=1000, nulls=0.1, n_metrics=4)

Generate a synthetic dataframe example with a variety of column types.

This can be used to showcase dataframes containing strings,
dates and floats, columns containing null values, and strongly
correlated columns.

Contains the following columns:
uid: A random identifying string of characters.
cities: A city randomly picked in a list of 20, or a null value.
encoded_cities: Ordinal encoding applied to the previous column.
start: A datetime.
end: A datetime later than the previous, or a null value.
metric_1, metric_2, etc: Randomly chosen float values.

* **Parameters:**
  **seed**
  : Seed for random generation.

  **size**
  : Number of rows in the output.

  **nulls**
  : Probability of a cell in ‘cities’ or ‘end’ being null.

  **n_metrics**
  : Number of ‘metrics’ columns added.
* **Returns:**
  pandas dataframe
  : The randomly-generated dataframe, with `size` rows and
    `5 + n_metrics` columns.

### Examples

```pycon
>>> from skrub.datasets import toy_cities
>>> df = toy_cities(seed=5, size=3, n_metrics=2)
>>> df
          uid     cities  ...  metric_0  metric_1
0  IPbQyAGoYc  Stockholm  ...  0.227319  0.895448
1  otDvgcachZ     Vienna  ...  0.872195  0.018517
2  jHNmownYjU        NaN  ...  0.707496  0.001200
```

<!-- !! processed by numpydoc !! -->
