# choose_float

### skrub.choose_float(low, high, , log=False, n_steps=None, name=None, default=None)

A choice of floating-point numbers from a numeric range.

When a learner is fitted *without hyperparameter tuning*, the outcome of
this choice is the middle of the range (possibly on a `log` scale). Pass
a float as the `default` argument to set the default outcome.

* **Parameters:**
  **low**
  : The start of the range.

  **high**
  : Then end of the range.

  **log**
  : Whether sampling should be done on a logarithmic scale.

  **n_steps**
  : If not `None`, a grid of `n_steps` values across the range is
    defined and sampling is done on that grid. This can be useful to limit
    the number of unique values that get sampled, for example to improve
    the effectiveness of caching. However, it means a much more restricted
    space of possible values gets explored.

  **name**
  : If not `None`, `name` is used when displaying search results and
    can also be used to override the choice’s value by setting it in the
    environment containing a learner’s inputs.

  **default**
  : If provided, override the choice’s default value when hyperparameter
    search is not used. Otherwise the default value is the middle of the
    range (either on a linear or logarithmic scale depending on the value
    of `log`).
* **Returns:**
  numeric choice
  : An object representing this choice, which can be used in a skrub
    learner.

#### SEE ALSO
[`choose_bool`](skrub.choose_boolhtml.md#skrub.choose_bool)
: Construct a choice between False and True.

[`choose_int`](skrub.choose_inthtml.md#skrub.choose_int)
: Construct a choice of integers from a numeric range.

[`choose_from`](skrub.choose_fromhtml.md#skrub.choose_from)
: Construct a choice among several possible outcomes.

[`optional`](skrub.optionalhtml.md#skrub.optional)
: Choose between executing an operation or not.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div>
<!-- thumbnail-parent-div-close --></div>
