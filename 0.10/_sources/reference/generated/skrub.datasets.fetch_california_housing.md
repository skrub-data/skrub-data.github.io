# fetch_california_housing

### skrub.datasets.fetch_california_housing(data_home=None)

Fetches the california housing dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Description of the dataset:
: This dataset was obtained from the StatLib repository:
  [https://www.dcc.fc.up.pt/~ltorgo/Regression/cal_housing.html](https://www.dcc.fc.up.pt/~ltorgo/Regression/cal_housing.html)
  <br/>
  The target variable is the median house value for California districts,
  expressed in hundreds of thousands of dollars ($100,000).
  <br/>
  This dataset was derived from the 1990 U.S. census, using one row per census
  block group. A block group is the smallest geographical unit for which the U.S.
  Census Bureau publishes sample data (a block group typically has a population of
  600 to 3,000 people).
  <br/>
  A household is a group of people residing within a home. Since the average
  number of rooms and bedrooms in this dataset are provided per household, these
  columns may take surprisingly large values for block groups with few households
  and many empty houses, such as vacation resorts.
  <br/>
  It can be downloaded/loaded using the sklearn.datasets.fetch_california_housing
  function.
  Size on disk: 1.80MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    california_housing
    : A dataframe with the California housing data.
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
    : The path to the california housing CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div>
<!-- thumbnail-parent-div-close --></div>
