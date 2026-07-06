# fetch_videogame_sales

### skrub.datasets.fetch_videogame_sales(data_home=None)

Fetch the videogame sales dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is a regression use-case, where the single table contains information
about videogames such as the publisher and platform, and the goal is to
predict the number of sales worldwide. Size on disk: 1.8MB.

#### WARNING
The original dataset is ordered by decreasing number of sales. This
should be taken into account for cross-validation. Depending on the
desired setting, one might consider shuffling the rows or ordering by
publication year and splitting by year.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    videogame_sales
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : Target labels.
    <br/>
    metadata
    : A dictionary containing the name, source and target.
    <br/>
    path
    : The path to the videogame sales CSV file.

<!-- !! processed by numpydoc !! -->
