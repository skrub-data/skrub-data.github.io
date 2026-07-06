# choose_from

### skrub.choose_from(outcomes, , name=None)

A choice among several possible outcomes.

When a learner is used *without hyperparameter tuning*, the outcome of
this choice is the first value in the `outcomes` list or dict.

* **Parameters:**
  **outcomes**
  : The possible outcomes to choose from. If a dict, the values are the
    outcomes and the keys give them human-readable names used to display
    hyperparameter search grids and results.

  **name**
  : If not `None`, `name` is used when displaying search results and
    can also be used to override the choice’s value by setting it in the
    environment containing a learner’s inputs.
* **Returns:**
  Choice
  : An object representing this choice, which can be used in a skrub
    DataOps plan.

#### SEE ALSO
[`choose_bool`](skrub.choose_boolhtml.md#skrub.choose_bool)
: Construct a choice between False and True.

[`choose_float`](skrub.choose_floathtml.md#skrub.choose_float)
: Construct a choice of floating-point numbers from a numeric range.

[`choose_int`](skrub.choose_inthtml.md#skrub.choose_int)
: Construct a choice of integers from a numeric range.

[`optional`](skrub.optionalhtml.md#skrub.optional)
: Choose between executing an operation or not.

### Examples

Outcomes can be provided in a list:

```pycon
>>> from skrub import choose_from
>>> choose_from([1, 2], name='the number')
choose_from([1, 2], name='the number')
```

They can also be provided in a dictionary to give a name to each outcome:

```pycon
>>> choose_from({'one': 1, 'two': 2}, name='the number')
choose_from({'one': 1, 'two': 2}, name='the number')
```

When a skrub learner containing a `choose_from` is fitted *without
hyperparameter tuning*, the default outcome for the choice is used.
It is the first outcome in the provided list or dict:

```pycon
>>> choose_from([1, 2, 3]).default()
1
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to wrap a PyTorch model with skorch and plug it into a skrub DataOps plan.">  <div class="sphx-glr-thumbnail-title">Using PyTorch (via skorch) in DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
