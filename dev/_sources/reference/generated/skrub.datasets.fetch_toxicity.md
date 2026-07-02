# fetch_toxicity

### skrub.datasets.fetch_toxicity(data_home=None)

Fetch the toxicity dataset (classification) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

> This is a balanced binary classification use-case, where the single table
> consists in only two columns:
- `text`: the text of the comment
- `is_toxic`: whether or not the comment is toxic

> Size on disk: 220KB.
* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    toxicity
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target.
    <br/>
    y
    : Target labels.
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    path
    : The path to the toxicity CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
