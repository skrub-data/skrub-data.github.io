# fetch_drug_directory

### skrub.datasets.fetch_drug_directory(data_home=None)

Fetches the drug directory dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Description of the dataset:
: Product listing data submitted to the U.S. FDA for all unfinished,
  unapproved drugs. Size on disk: 44MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    drug_directory
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : The target labels.
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    path
    : The path to the drug directory CSV file.

<!-- !! processed by numpydoc !! -->
