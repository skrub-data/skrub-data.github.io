# fetch_bike_sharing

### skrub.datasets.fetch_bike_sharing(data_home=None)

Fetch the bike sharing dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is a regression use-case, where the goal is to predict demand for a
bike-sharing service. The features are the dates and holiday and weather
information. Size on disk: 1.3MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    bike_sharing
    : The full dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : Target labels.
    <br/>
    metadata
    : A dictionary containing the name and target.
    <br/>
    path
    : The path to the bike sharing CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we illustrate how to better integrate datetime features in machine learning models with the |DatetimeEncoder|.">  <div class="sphx-glr-thumbnail-title">Handling datetime features with the DatetimeEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>
