# XySkrubLearner

### *class* skrub.XySkrubLearner(learner, X='X', y='y')

Wrapper to expose a standard scikit-learn interface to a skrub learner.

This class is not usually instantiated manually, but by calling
[`.with_sklearn_interface()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.with_sklearn_interface)
on a skrub learner. Skrub learners are created from a [`DataOp`](skrub.DataOphtml.md#skrub.DataOp) with
[`.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner),
[`.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search),
etc.

The wrapped learner can be retrieved from the `learner` attribute.

* **Parameters:**
  **learner**
  : The learner to wrap.

  **X**
  : Name or ID of the node that receives the `X` argument.

  **y**
  : Name or ID of the node that receives the `y` argument.

#### SEE ALSO
[`SkrubLearner.with_sklearn_interface`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.with_sklearn_interface)
: Create a `XySkrubLearner`. See this method for all the details.

### Notes

See the [user guide](../../modules/data_ops/validation/exporting_data_opshtml.md#user-guide-data-ops-sklearn-interface) and this
[example](../../auto_examples/02_data_ops/1170_sklearn_interface_learnerhtml.md#example-with-sklearn-interface) for more information.

### Methods

| [`get_param_grid`](#skrub.XySkrubLearner.get_param_grid)([key_prefix, outer_grid])   | Get a param grid for this learner (use with GridSearchCV, RandomizedSearchCV).   |
|--------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [`get_params`](#skrub.XySkrubLearner.get_params)([deep])                             | Get parameters for this estimator.                                               |
| [`set_params`](#skrub.XySkrubLearner.set_params)(\*\*params)                         | Set the parameters of this estimator.                                            |

| **get_named_params**   |    |
|------------------------|----|
| **set_named_params**   |    |
<!-- !! processed by numpydoc !! -->

#### get_param_grid(key_prefix='', outer_grid=None)

Get a param grid for this learner (use with GridSearchCV, RandomizedSearchCV).

#### NOTE
The recommended way to tune hyperparameters is to use the skrub
DataOp methods [`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search),
[`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) rather than the scikit-learn
meta-estimators.

This method is meant to be used when the learner is a plain
[`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner), i.e. it was created with
[`DataOp.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner). When the learner is itself a skrub
hyperparameter search (e.g. created with
[`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)), it does its own internal
tuning and from an external point of view it has no hyperparameters to
tune, in this case `outer_grid` is returned unchanged.

* **Parameters:**
  **key_prefix**
  : Keys in the grid are prefixed with this string. Typically this is
    the path to reach the learner in a metaestimator such as a
    pipeline, for example `'vectorizer__'` if the learner is in a
    pipeline step named `'vectorizer'`.

  **outer_grid**
  : Outer param grid for other steps in a pipeline. This function
    returns an augmented parameter grid in which the learner’s
    parameter ranges have been inserted.
* **Returns:**
  [`list`](https://docs.python.org/3/builtins/stdtypes.html#list) of [`dict`](https://docs.python.org/3/builtins/stdtypes.html#dict)
  : The parameter grid. If the wrapped learner is a hyperparameter
    search ([`ParamSearch`](skrub.ParamSearchhtml.md#skrub.ParamSearch) or [`OptunaParamSearch`](skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch)), it
    does its own tuning and adds nothing to the provided grid.

### Examples

```pycon
>>> import skrub
>>> from sklearn.pipeline import Pipeline
>>> from sklearn.linear_model import Ridge
```

```pycon
>>> encoder = (
...     skrub.var("X")
...     .skb.apply(
...         skrub.StringEncoder(
...             n_components=skrub.choose_from([10, 20, 30], name="n_components")
...         )
...     )
...     .skb.make_learner()
...     .with_sklearn_interface()
... )
```

```pycon
>>> pipe = Pipeline([("encoder", encoder), ("ridge", Ridge())])
>>> outer_grid = {"ridge__alpha": [.1, 1., 10.]}
```

We obtain a grid that can be passed to GridSearchCV that can be used for jointly
tuning the parameters of the Ridge and skrub learner:

```pycon
>>> encoder.get_param_grid("encoder__", outer_grid)
[{'ridge__alpha': [0.1, 1.0, 10.0], 'encoder__0:n_components': ['0:10', '1:20', '2:30']}]
```

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
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="When our DataOp only needs 2 inputs, X and y, we can get a learner that accepts the usual scikit-learn inputs rather than a dictionary like SkrubLearners normally do, by using SkrubLearner.with_sklearn_interface to obtain the learner wrapped in a XySkrubLearner.">  <div class="sphx-glr-thumbnail-title">Turn a DataOp into a "standard" scikit-learn estimator</div>
</div>
<!-- thumbnail-parent-div-close --></div>
