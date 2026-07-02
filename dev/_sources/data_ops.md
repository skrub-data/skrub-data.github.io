<a id="user-guide-data-ops-index"></a>

# Building complete pipelines with DataOps

A skrub DataOp is a complete machine learning pipeline —from data loading and
wrangling to the final prediction— in a single object that can be fitted, tuned,
cross-validated, and saved in a file like any scikit-learn estimator.

By integrating the whole data processing, DataOps help to validate pipelines
while **avoiding data leakage**, to **tune complex modelling choices**, and to keep
track of important **fitted (learned) state**.

To solve a machine-learning task we often need to combine multiple operations
such as loading and filtering data, joining tables and computing aggregations,
extracting numerical features, and fitting a classifier or regressor.

**Storing state**  Each of those operations may need to be fitted: to learn some
information from training data and reuse it to apply consistent transformations
to new data. This is the case for transformers like the
[`StandardScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) and [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and
estimators like [`RandomForestClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier).

**Tuning**  Moreover, each processing step may involve decisions that need to be
tuned (*tuning* means finding the value that gives the best predictive
performance), for example: what weather forecast features should I include to
predict the load on an electric grid? How should I encode a product description
to help predict the product’s category? What learning rate to set on a
[`HistGradientBoostingRegressor`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html#sklearn.ensemble.HistGradientBoostingRegressor)?

**Validation**  Finally, the quality of predictions must be evaluated on
held-out data (with a train/test split or cross-validation), taking care to
**avoid leakage** of test data into the training set.

Separating the data wrangling from the fitted estimator prevents correctly
handling the tasks above. Skrub DataOps help by binding an arbitrary set of
transformations of any number of inputs in a single estimator. These
transformations can be easily parametrized with tunable choices. The resulting
objects have built-in methods for cross-validation and tuning with either Optuna
or scikit-learn, and for inspecting runs and intermediate results. Once fitted,
they can be saved in a file, loaded, applied to new data as easily as a single
[`LogisticRegression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression).

### Going beyond the scikit-learn Pipeline

To some extent, the DataOps exist for the same reasons as the simpler
scikit-learn [`sklearn.pipeline.Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline) used in other parts of this
documentation. However the Pipeline is too limited for many real-world problems:
it can only represent a linear sequence of scikit-learn transformers, the design
matrix and target variables must be constructed and divided into training and
testing sets outside of the pipeline and the number of rows cannot change, only
a single table can be handled, hyperparameter choices are difficult to define,
etc. . Skrub DataOps remove those limitations and add several useful features
such as interactive previews and integration with Optuna.

## A quick overview of DataOps

This tutorial walks through the main components of the DataOps on a simple
example:

* [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tourhtml.md)

## Data Ops basic concepts

* [Basics of DataOps: the DataOps plan, variables, and learners](modules/data_ops/basics/what_are_data_opshtml.md)
* [Building a simple DataOps plan](modules/data_ops/basics/building_data_ops_planhtml.md)
* [Using previews for easier development and debugging](modules/data_ops/basics/using_previewshtml.md)
  * [Defining a default value for a variable](modules/data_ops/basics/using_previewshtml.md#defining-a-default-value-for-a-variable)
  * [Disabling previews and eager checks](modules/data_ops/basics/using_previewshtml.md#disabling-previews-and-eager-checks)
* [DataOps allow direct access to methods of the underlying data](modules/data_ops/basics/direct_access_methodshtml.md)
* [Control flow in DataOps: eager and deferred evaluation](modules/data_ops/basics/control_flowhtml.md)
  * [Unpacking multiple outputs from deferred functions](modules/data_ops/basics/control_flowhtml.md#unpacking-multiple-outputs-from-deferred-functions)
* [How do skrub Data Ops differ from the alternatives?](modules/data_ops/basics/data_ops_vs_alternativeshtml.md)
  * [Skrub DataOps and scikit-learn `sklearn.pipeline.Pipeline`](modules/data_ops/basics/data_ops_vs_alternativeshtml.md#skrub-dataops-and-scikit-learn-sklearn-pipeline-pipeline)
  * [Skrub DataOps and orchestrators like Apache Airflow](modules/data_ops/basics/data_ops_vs_alternativeshtml.md#skrub-dataops-and-orchestrators-like-apache-airflow)
  * [Skrub DataOps and other skrub objects, like `tabular_pipeline()`](modules/data_ops/basics/data_ops_vs_alternativeshtml.md#skrub-dataops-and-other-skrub-objects-like-tabular-pipeline)
  * [Can I use library “x” with skrub DataOps?](modules/data_ops/basics/data_ops_vs_alternativeshtml.md#can-i-use-library-x-with-skrub-dataops)

## Building a complex pipeline with the skrub Data Ops

* [Applying machine-learning estimators](modules/data_ops/ml_pipeline/applying_ml_estimatorshtml.md)
* [Applying different transformers using skrub selectors and DataOps](modules/data_ops/ml_pipeline/applying_different_transformershtml.md)
* [Documenting the DataOps plan with node names and descriptions](modules/data_ops/ml_pipeline/documenting_data_ops_planhtml.md)
* [Evaluating and debugging the DataOps plan with `.skb.full_report()`](modules/data_ops/ml_pipeline/evaluating_debugging_data_opshtml.md)
* [Using only a part of a DataOps plan](modules/data_ops/ml_pipeline/using_part_of_data_ops_planhtml.md)
* [Subsampling data for easier development and debugging](modules/data_ops/ml_pipeline/subsampling_datahtml.md)

## Tuning and validating skrub DataOps plans

* [Tuning and validating skrub DataOps plans](modules/data_ops/validation/tuning_validating_data_opshtml.md)
* [Improving the confidence in our score through cross-validation](modules/data_ops/validation/tuning_validating_data_opshtml.md#improving-the-confidence-in-our-score-through-cross-validation)
* [Splitting the data in train and test sets](modules/data_ops/validation/tuning_validating_data_opshtml.md#splitting-the-data-in-train-and-test-sets)
* [Passing additional arguments to the splitter](modules/data_ops/validation/tuning_validating_data_opshtml.md#passing-additional-arguments-to-the-splitter)
* [Passing additional arguments to the scorer](modules/data_ops/validation/tuning_validating_data_opshtml.md#passing-additional-arguments-to-the-scorer)
* [Avoiding computing predictions multiple times](modules/data_ops/validation/tuning_validating_data_opshtml.md#avoiding-computing-predictions-multiple-times)
* [Using the skrub `choose_*` functions to tune hyperparameters](modules/data_ops/validation/hyperparameter_tuninghtml.md)
* [Feature selection with skrub `SelectCols` and `DropCols`](modules/data_ops/validation/hyperparameter_tuninghtml.md#feature-selection-with-skrub-selectcols-and-dropcols)
* [Validating hyperparameter search with nested cross-validation](modules/data_ops/validation/nested_cross_validationhtml.md)
* [Going beyond estimator hyperparameters: nesting choices and choosing pipelines](modules/data_ops/validation/nesting_choices_choosing_pipelineshtml.md)
* [Linking choices depending on other choices](modules/data_ops/validation/nesting_choices_choosing_pipelineshtml.md#linking-choices-depending-on-other-choices)
* [Exporting the DataOps plan as a learner and reusing it](modules/data_ops/validation/exporting_data_opshtml.md)
* [Tuning DataOps with Optuna](modules/data_ops/validation/tuning_with_optunahtml.md)
  * [Using Optuna as a backend for randomized search](modules/data_ops/validation/tuning_with_optunahtml.md#using-optuna-as-a-backend-for-randomized-search)
  * [Setting a storage for the Optuna study](modules/data_ops/validation/tuning_with_optunahtml.md#setting-a-storage-for-the-optuna-study)
  * [Using Optuna directly](modules/data_ops/validation/tuning_with_optunahtml.md#using-optuna-directly)
  * [Parallelism](modules/data_ops/validation/tuning_with_optunahtml.md#parallelism)
  * [Using the Optuna dashboard](modules/data_ops/validation/tuning_with_optunahtml.md#using-the-optuna-dashboard)
