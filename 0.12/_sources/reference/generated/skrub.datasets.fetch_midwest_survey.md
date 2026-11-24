# fetch_midwest_survey

### skrub.datasets.fetch_midwest_survey(data_home=None)

Fetches the midwest survey dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Survey to know if people self-identify as Midwesterners. Size on disk: 504KB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    midwest_survey
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : Target labels,
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    path
    : The path to the midwest survey CSV file.

### Examples

```pycon
>>> from skrub.datasets import fetch_midwest_survey
>>> data = fetch_midwest_survey()
>>> print(data.keys())
dict_keys(['path', 'metadata', 'metadata_path', 'midwest_survey',
  'midwest_survey_path', 'X', 'y'])
```

<!-- !! processed by numpydoc !! -->
