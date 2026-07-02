# fetch_credit_fraud

### skrub.datasets.fetch_credit_fraud(data_home=None, split='train')

Fetch the credit fraud dataset (classification).

Available at [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is an imbalanced binary classification use-case. This dataset consists of
two tables:

- baskets, containing the binary fraud target label
- products

Baskets contain at least one product each, so aggregation then joining operations
are required to build a design matrix.
Size on disk: 16MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.

  **split**
  : The split to load. Can be either “train”, “test”, or “all”.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    baskets
    : Table containing baskets ID and target.
    <br/>
    products
    : Table containing features about products contained in baskets
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    baskets_path
    : The path to the baskets CSV file.
    <br/>
    products_path
    : The path to the products CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
