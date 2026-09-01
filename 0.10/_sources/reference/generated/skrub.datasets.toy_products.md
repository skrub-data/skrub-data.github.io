# toy_products

### skrub.datasets.toy_products()

Generate a synthetic dataframe example of a mock product catalog.

Contains the following columns:
description: A brief description of the product.
price: The price of the product.
seller: The name of the seller.
category: The category to which the product belongs.

* **Returns:**
  pandas Dataframe
  : The synthetic dataframe of shape (6, 4), with columns labelled description,
    price, seller, and category.

### Examples

```pycon
>>> from skrub.datasets import toy_products
>>> df = toy_products()
>>> df.shape
(6, 4)
```

<!-- !! processed by numpydoc !! -->
