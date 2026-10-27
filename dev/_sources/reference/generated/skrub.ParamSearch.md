# ParamSearch

### *class* skrub.ParamSearch(data_op, search)

Learner that evaluates a skrub DataOp with hyperparameter tuning.

This class is not meant to be instantiated manually, `ParamSearch`
objects are created by calling [`make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) or
[`make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search) on a DataOp.

* **Attributes:**
  **classes_**

  [`detailed_results_`](#skrub.ParamSearch.detailed_results_)
  : More detailed cross-validation results table.

  [`results_`](#skrub.ParamSearch.results_)
  : Cross-validation results containing parameters and scores in a dataframe.

### Methods

| [`get_params`](#skrub.ParamSearch.get_params)([deep])                               | Get parameters for this estimator.                                 |
|-------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| [`plot_results`](#skrub.ParamSearch.plot_results)(\*[, colorscale, min_score, ...]) | Create a parallel coordinate plot of the cross-validation results. |
| [`set_params`](#skrub.ParamSearch.set_params)(\*\*params)                           | Set the parameters of this estimator.                              |

| **fit**   |    |
|-----------|----|
<!-- !! processed by numpydoc !! -->

#### *property* detailed_results_

More detailed cross-validation results table.

Similar to `results_` but also contains the standard deviation of
scores across folds, the scores on training data, and the fit and
score durations.

<!-- !! processed by numpydoc !! -->

#### get_params(deep=True)

Get parameters for this estimator.

* **Parameters:**
  **deep**
  : If True, will return the parameters for this estimator and
    contained subobjects that are estimators.
* **Returns:**
  **params**
  : Parameter names mapped to their values.

<!-- !! processed by numpydoc !! -->

#### plot_results(, colorscale='bluered', min_score=None, show_scores=None, show_choices=None, show_times=None)

Create a parallel coordinate plot of the cross-validation results.

Plotly must be installed to use this method.

* **Parameters:**
  **colorscale**
  : A colorscale name understood by plotly.

  **min_score**
  : Lines for models that have a score lower than `min_score` are not
    displayed, and the color scale lower bound is adjusted accordingly.
    This is useful when we are only interested in the models that
    perform well, to make the plot less cluttered and to make better
    use of the colorscale’s range.

  **show_scores**
  : List of score names to show
    (e.g. “accuracy” in `.skb.with_scoring(["roc_auc", "accuracy"])`).
    By default all are shown.

  **show_choices**
  : List of choice names to show
    (e.g. “alpha” in `choose_float(0.0, 1.0, name="alpha")`).
    Only choices that were given an explicit name (with the `name`
    parameter, as above) can be selected by the filter.
    By default there is no filtering and all choices (even those
    without names) are shown.

  **show_times**
  : List of durations to show. Available times are [“fit”, “score”].
    By default both are shown.
* **Returns:**
  Plotly [`Figure`](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.html#matplotlib.figure.Figure)

<!-- !! processed by numpydoc !! -->

#### *property* results_

Cross-validation results containing parameters and scores in a dataframe.

<!-- !! processed by numpydoc !! -->

#### set_params(\*\*params)

Set the parameters of this estimator.

The method works on simple estimators as well as on nested objects
(such as [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)). The latter have
parameters of the form `<component>__<parameter>` so that it’s
possible to update each component of a nested object.

* **Parameters:**
  **\*\*params**
  : Estimator parameters.
* **Returns:**
  **self**
  : Estimator instance.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to wrap a PyTorch model with skorch and plug it into a skrub DataOps plan.">  <div class="sphx-glr-thumbnail-title">Using PyTorch (via skorch) in DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
