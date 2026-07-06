# fetch_employee_salaries

### skrub.datasets.fetch_employee_salaries(data_home=None, split='all')

Fetches the employee salaries dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Description of the dataset:
: Annual salary information including gross pay and overtime pay for all
  active, permanent employees of Montgomery County, MD paid in calendar
  year 2016. This dataset is a copy of [https://www.openml.org/d/42125](https://www.openml.org/d/42125)
  where some features are dropped to avoid data leaking.
  Size on disk: 1.3MB.

#### NOTE
Some environments like Jupyterlite can run into networking issues when
connecting to a remote server, but OpenML provides CORS headers. To
download this dataset using OpenML instead of Github or Figshare, run:

```python
from sklearn.datasets import fetch_openml
df = fetch_openml(data_id=42125)
```

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.

  **split**
  : The split to load. Can be either “train”, “test”, or “all”.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    employee_salaries
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
    : The path to the employee salaries CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="The following example illustrates the use of the SquashingScaler, a transformer that can rescale and squash numerical features to a range that works well with neural networks and perhaps also other related models. Its basic idea is to rescale the features based on quantile statistics (to be robust to outliers), and then perform a smooth squashing function to limit the outputs to a pre-defined range. This transform has been found to even work well when applied to one-hot encoded features.">  <div class="sphx-glr-thumbnail-title">SquashingScaler: Robust numerical preprocessing for neural networks</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to use DataOp.skb.subsample to speed up interactive construction of a skrub DataOps plan by computing previews on a subsampled version of the original data.">  <div class="sphx-glr-thumbnail-title">Subsampling for faster development</div>
</div>
<!-- thumbnail-parent-div-close --></div>
