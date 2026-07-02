# toy_orders

### skrub.datasets.toy_orders(split='train')

Create a toy dataframe and corresponding targets for examples.

* **Parameters:**
  **split**
  : The split to load. Can be either “train”, “test”, or “all”.
* **Returns:**
  **bunch**
  : A dictionary-like object with the keys ‘X’, ‘y’ and ‘orders’.

### Examples

```pycon
>>> from skrub.datasets import toy_orders
>>> toy_orders(split="train").X
ID  product quantity        date
0   1       pen     2       2020-04-03
1   2       cup     3       2020-04-04
2   3       cup     5       2020-04-04
3   4       spoon   1       2020-04-05
>>> toy_orders(split="train").y
0    False
1    False
2     True
3    False
Name: delayed, dtype: bool
```

If you want both X and y in a dataframe, use `.orders`:

```pycon
>>> toy_orders().orders
ID  product quantity        date    delayed
0   1       pen     2       2020-04-03      False
1   2       cup     3       2020-04-04      False
2   3       cup     5       2020-04-04      True
3   4       spoon   1       2020-04-05      False
```

<!-- !! processed by numpydoc !! -->
