# ParamSearch

### *class* skrub.ParamSearch(data_op, search)

Learner that evaluates a skrub DataOp with hyperparameter tuning.

This class is not meant to be instantiated manually, `ParamSearch`
objects are created by calling [`make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) or
[`make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search) on a DataOp.

Its methods accept a dictionary of inputs rather than `X` and `y`. See
[`ParamSearch.with_sklearn_interface()`](#skrub.ParamSearch.with_sklearn_interface) to obtain an estimator with the
standard scikit-learn interface.

* **Attributes:**
  **classes_**

  [`detailed_results_`](#skrub.ParamSearch.detailed_results_)
  : More detailed cross-validation results table.

  [`results_`](#skrub.ParamSearch.results_)
  : Cross-validation results containing parameters and scores in a dataframe.

### Methods

| [`get_params`](#skrub.ParamSearch.get_params)([deep])                               | Get parameters for this estimator.                                              |
|-------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| [`plot_results`](#skrub.ParamSearch.plot_results)(\*[, colorscale, min_score, ...]) | Create a parallel coordinate plot of the cross-validation results.              |
| [`set_params`](#skrub.ParamSearch.set_params)(\*\*params)                           | Set the parameters of this estimator.                                           |
| [`with_sklearn_interface`](#skrub.ParamSearch.with_sklearn_interface)([X, y])       | Wrap this learner in an estimator exposing the standard scikit-learn interface. |

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

#### with_sklearn_interface(X='X', y='y')

Wrap this learner in an estimator exposing the standard scikit-learn interface.

Learners created from a [`DataOp`](skrub.DataOphtml.md#skrub.DataOp) with [`.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner), [`.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search), etc. accept their inputs as a
dictionary, because there are no restrictions on the number and type of
inputs they can process:
`learner.fit({"some_input": ..., "some_other_input": ..., ...})`.

When we need the standard scikit-learn interface `fit(X, y)` we can
wrap our learner in a [`XySkrubLearner`](skrub.XySkrubLearnerhtml.md#skrub.XySkrubLearner) by calling
[`SkrubLearner.with_sklearn_interface()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.with_sklearn_interface). The returned estimator
accepts X, y and kwargs and those are put in a dictionary that is
passed to the wrapped SkrubLearner (or ParamSearch, or OptunaParamSearch).

The parameters `X` and `y` give the names of variables that should
be bound to the `X` and `y` arguments passed to `fit`,
`predict`, etc. (by default “X” and “y”).

* **Parameters:**
  **X**
  : [`Name`](skrub.DataOp.skb.namehtml.md#skrub.DataOp.skb.name) or [`ID`](skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id) of the
    node (usually a [`variable`](skrub.varhtml.md#skrub.var)) in the DataOp that
    receives the `X` argument.

  **y**
  : [`Name`](skrub.DataOp.skb.namehtml.md#skrub.DataOp.skb.name) or [`ID`](skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id) of the
    node (usually a [`variable`](skrub.varhtml.md#skrub.var)) in the DataOp that
    receives the `y` argument. Only required if the DataOp uses a `y`.
    <br/>
    (Passing `None` explicitly disables passing any `y` even if one
    is provided to the estimator method. Usually leaving the default is
    fine even for DataOps that do not use a `y`.)
* **Returns:**
  [`skrub.XySkrubLearner`](skrub.XySkrubLearnerhtml.md#skrub.XySkrubLearner)
  : A wrapper that adapts this learner to the standard scikit-learn
    `fit(X, y)` interface.

#### SEE ALSO
[`XySkrubLearner`](skrub.XySkrubLearnerhtml.md#skrub.XySkrubLearner)
: The class of the returned estimator.

[`XySkrubLearner.get_param_grid`](skrub.XySkrubLearnerhtml.md#skrub.XySkrubLearner.get_param_grid)
: Param grid for tuning the learner’s choices with scikit-learn’s `GridSearchCV` or `RandomizedSearchCV`.

[`DataOp.skb.make_learner`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)
: Get a skrub learner for a DataOp.

### Notes

The returned estimator can be used wherever a scikit-learn estimator is
expected, for example in a [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline), with
[`sklearn.model_selection.cross_validate()`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate), or as the `estimator`
in [`DataOp.skb.apply()`](skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) to nest the DataOp in another one. Skrub
functions that expect a skrub learner, such as
[`skrub.cross_validate()`](skrub.cross_validatehtml.md#skrub.cross_validate), should be given the original learner
instead.

See also the [user guide](../../modules/data_ops/validation/exporting_data_opshtml.md#user-guide-data-ops-sklearn-interface)
and this [example](../../auto_examples/02_data_ops/1170_sklearn_interface_learnerhtml.md#example-with-sklearn-interface).

### Examples

We show examples with [`.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner) here, which returns a [`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner),
but this method works similarly for [`ParamSearch`](#skrub.ParamSearch) and
[`OptunaParamSearch`](skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) (created by [`.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) and [`.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)).

```pycon
>>> import sklearn
>>> from sklearn.utils.fixes import parse_version
>>> if parse_version(sklearn.__version__) < parse_version("1.9"):
...     import pytest
...     raise pytest.skip()
>>> import skrub
>>> from sklearn.preprocessing import TargetEncoder
>>> from sklearn.model_selection import KFold
```

**Example data**

```pycon
>>> data = skrub.datasets.toy_orders()
>>> orders_df, delayed_col = data["X"], data["y"]
>>> orders_df
   ID product  quantity        date
0   1     pen         2  2020-04-03
1   2     cup         3  2020-04-04
2   3     cup         5  2020-04-04
3   4   spoon         1  2020-04-05
```

```pycon
>>> delayed_col
0    False
1    False
2     True
3    False
Name: delayed, dtype: bool
```

**Skrub learner and scikit-learn adapter**

Let us define a DataOp that preprocesses the orders dataframe,
and build the corresponding skrub learner:

```pycon
>>> cv = KFold(2)
>>> learner = (
...     skrub.var("X")
...     .skb.apply(skrub.Cleaner())
...     .skb.apply(skrub.DatetimeEncoder(add_total_seconds=False), cols="date")
...     .skb.apply(TargetEncoder(cv=cv), y=skrub.var("y"), cols="product")
... ).skb.make_learner()
```

The SkrubLearner accepts an ‘environment’ dictionary, containing
values for all the variables in the DataOp (here, X and y):

```pycon
>>> learner.fit_transform({"X": orders_df, "y": delayed_col})
   ID  quantity  date_year  date_month  date_day  product
0   1         2     2020.0         4.0       3.0      0.5
1   2         3     2020.0         4.0       4.0      1.0
2   3         5     2020.0         4.0       4.0      0.0
3   4         1     2020.0         4.0       5.0      0.0
```

If we need an estimator that has the standard scikit-learn
interface, we can use `with_sklearn_interface()`:

```pycon
>>> learner.with_sklearn_interface().fit_transform(orders_df, delayed_col)
   ID  quantity  date_year  date_month  date_day  product
0   1         2     2020.0         4.0       3.0      0.5
1   2         3     2020.0         4.0       4.0      1.0
2   3         5     2020.0         4.0       4.0      0.0
3   4         1     2020.0         4.0       5.0      0.0
```

In this case the `X` and `y` passed to `fit_transform` are
mapped to the `"X"` and `"y"` keys in the environment.

If the DataOp contains no supervised estimators, `y` can be
omitted. Here `y` is needed because the TargetEncoder requires
it.

**The learner and its wrapper share state.**

The estimator returned by `with_sklearn_interface` wraps the
original learner (and not a copy), so any fitted attributes are
shared. Thus it can also be obtained from an already-fitted
model. Here our `learner` has been fitted, so we can call
`transform`:

```pycon
>>> learner.with_sklearn_interface().transform(orders_df, delayed_col)
   ID  quantity  date_year  date_month  date_day  product
0   1         2     2020.0         4.0       3.0      0.0
1   2         3     2020.0         4.0       4.0      0.4
2   3         5     2020.0         4.0       4.0      0.4
3   4         1     2020.0         4.0       5.0      0.0
```

The wrapped learner can be obtained from the `learner`
attribute:

```pycon
>>> learner.with_sklearn_interface().learner is learner
True
```

**Using different variable names than X and y.**

If the names of our variables are different than `"X"` and
`"y"`, we can specify the names to use in the call to
`with_sklearn_interface`. For example here we use the names
`"orders"` and `"delayed"` instead:

```pycon
>>> learner = (
...     skrub.var("orders")
...     .skb.apply(skrub.Cleaner())
...     .skb.apply(skrub.DatetimeEncoder(add_total_seconds=False), cols="date")
...     .skb.apply(TargetEncoder(cv=cv), y=skrub.var("delayed"), cols="product")
... ).skb.make_learner()
```

```pycon
>>> learner.fit_transform({"orders": orders_df, "delayed": delayed_col})
   ID  quantity  date_year  date_month  date_day  product
0   1         2     2020.0         4.0       3.0      0.5
1   2         3     2020.0         4.0       4.0      1.0
2   3         5     2020.0         4.0       4.0      0.0
3   4         1     2020.0         4.0       5.0      0.0
```

```pycon
>>> learner.with_sklearn_interface(X="orders", y="delayed").fit_transform(
...     orders_df, delayed_col
... )
   ID  quantity  date_year  date_month  date_day  product
0   1         2     2020.0         4.0       3.0      0.5
1   2         3     2020.0         4.0       4.0      1.0
2   3         5     2020.0         4.0       4.0      0.0
3   4         1     2020.0         4.0       5.0      0.0
```

Moreover, the names to which we map `X` and `y` are not required to
belong to skrub variables (created with [`var()`](skrub.varhtml.md#skrub.var), shown here).
They can be any type of node with a name (set with
[`.skb.set_name()`](skrub.DataOp.skb.set_namehtml.md#skrub.DataOp.skb.set_name)), or they can also be a
node ID rather than a string (see [`.skb.id`](skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id)).

**Additional variables can be passed as kwargs.**

When our DataOp has more than those 2 inputs, additional data can
be passed to the sklearn interface through kwargs. Suppose we
have a third table containing information about the products:

```pycon
>>> products_df = data.products
>>> products_df
  product  price
0     cup      5
1     pen      2
2   spoon      3
```

```pycon
>>> learner = (
...     skrub.var("orders")
...     .merge(skrub.var("products"), on="product")  # ⟵ Add the price information
...     .skb.apply(skrub.Cleaner())
...     .skb.apply(skrub.DatetimeEncoder(add_total_seconds=False), cols="date")
...     .skb.apply(TargetEncoder(cv=cv), y=skrub.var("delayed"), cols="product")
... ).skb.make_learner()
```

In the usual SkrubLearner interface, bindings for all the
variables are passed in the environment:

```pycon
>>> learner.fit_transform(
...     {"orders": orders_df, "delayed": delayed_col, "products": products_df}
... )
   ID  quantity  date_year  date_month  date_day  price  product
0   1         2     2020.0         4.0       3.0      2      0.5
1   2         3     2020.0         4.0       4.0      5      1.0
2   3         5     2020.0         4.0       4.0      5      0.0
3   4         1     2020.0         4.0       5.0      3      0.0
```

In the scikit-learn-compatible wrapper, additional inputs are
passed as named arguments (the name corresponds to the variable
name, here “products”):

```pycon
>>> learner.with_sklearn_interface(X="orders", y="delayed").fit_transform(
...     orders_df, delayed_col, products=products_df
... )
   ID  quantity  date_year  date_month  date_day  price  product
0   1         2     2020.0         4.0       3.0      2      0.5
1   2         3     2020.0         4.0       4.0      5      1.0
2   3         5     2020.0         4.0       4.0      5      0.0
3   4         1     2020.0         4.0       5.0      3      0.0
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to wrap a PyTorch model with skorch and plug it into a skrub DataOps plan.">  <div class="sphx-glr-thumbnail-title">Using PyTorch (via skorch) in DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
