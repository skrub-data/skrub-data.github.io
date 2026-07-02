# OptunaParamSearch

### *class* skrub.OptunaParamSearch(data_op, n_iter=10, scoring=None, n_jobs=None, refit=True, cv=None, verbose=0, pre_dispatch='2\*n_jobs', random_state=None, error_score=nan, return_train_score=False, storage=None, study_name=None, sampler=None, timeout=None)

Learner that evaluates a skrub DataOp with hyperparameter tuning.

This class is not meant to be instantiated manually, `OptunaParamSearch`
objects are created by calling
[`.skb.make_randomized_search(backend='optuna')`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search) on a [`DataOp`](skrub.DataOphtml.md#skrub.DataOp).

Attributes of interest are `best_learner_`, `best_score_`,
`results_`, `detailed_results_`, and `study_` (the Optuna study used
to find hyperparameters).

* **Attributes:**
  **classes_**

  [`detailed_results_`](#skrub.OptunaParamSearch.detailed_results_)
  : More detailed cross-validation results table.

  [`results_`](#skrub.OptunaParamSearch.results_)
  : Cross-validation results containing parameters and scores in a dataframe.

### Methods

| [`get_params`](#skrub.OptunaParamSearch.get_params)([deep])                          | Get parameters for this estimator.                                 |
|--------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| [`plot_results`](#skrub.OptunaParamSearch.plot_results)(\*[, colorscale, min_score]) | Create a parallel coordinate plot of the cross-validation results. |
| [`set_params`](#skrub.OptunaParamSearch.set_params)(\*\*params)                      | Set the parameters of this estimator.                              |

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

#### plot_results(, colorscale='bluered', min_score=None)

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
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div>
<!-- thumbnail-parent-div-close --></div>
