# fetch_traffic_violations

### skrub.datasets.fetch_traffic_violations(data_home=None)

Fetches the traffic violations dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Description of the dataset:
: This dataset contains traffic violation information from all electronic
  traffic violations issued in the Montgomery County, MD. Any information
  that can be used to uniquely identify the vehicle, the vehicle owner or
  the officer issuing the violation will not be published. Size on disk: 736MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    traffic_violations
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
    : The path to the traffic violations CSV file.

<!-- !! processed by numpydoc !! -->
