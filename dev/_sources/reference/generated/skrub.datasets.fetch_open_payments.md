# fetch_open_payments

### skrub.datasets.fetch_open_payments(data_home=None)

Fetches the open payments dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Payments given by healthcare manufacturing companies to medical doctors
or hospitals. Size on disk: 8.7MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    open_payments
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : Target labels.
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    path
    : The path to the open payments CSV file.

### Examples

```pycon
>>> from skrub.datasets import fetch_open_payments
>>> data = fetch_open_payments()
>>> print(data.keys())
dict_keys(['path', 'metadata', 'metadata_path', 'open_payments',
  'open_payments_path', 'X', 'y'])
```

<!-- !! processed by numpydoc !! -->
