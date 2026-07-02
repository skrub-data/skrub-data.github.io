<a id="user-guide-data-ops-hyperparameter-tuning"></a>

# Using the skrub `choose_*` functions to tune hyperparameters

skrub provides a convenient way to declare ranges of possible values, and tune
those choices to keep the values that give the best predictions on a validation
set.

Rather than specifying a grid of hyperparameters separately from the pipeline,
we simply insert special skrub objects in place of the value.

We define the same set of operations as before:

```pycon
>>> from sklearn.datasets import load_diabetes
>>> from sklearn.linear_model import Ridge
>>> import skrub
>>> diabetes_df = load_diabetes(as_frame=True)["frame"]
>>> data = skrub.var("data", diabetes_df)
>>> X = data.drop(columns="target", errors="ignore").skb.mark_as_X()
>>> y = data["target"].skb.mark_as_y()
>>> pred = X.skb.apply(Ridge(), y=y)
```

Now, we can
replace the hyperparameter `alpha` (which should be a float) with a range
created by [`skrub.choose_float()`](../../../reference/generated/skrub.choose_floathtml.md#skrub.choose_float). skrub can use it to select the best value
for `alpha`.

```pycon
>>> pred = X.skb.apply(
...     Ridge(alpha=skrub.choose_float(1e-6, 10.0, log=True, name="α")), y=y
... )
```

#### WARNING
When we do [`.skb.make_learner()`](../../../reference/generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner), the
pipeline we obtain does not perform any hyperparameter tuning. The pipeline
we obtain by default uses default values for each of the choices. For numeric
choices it is the middle of the range (unless an explicit default has been
set when creating the choice), and for [`choose_from()`](../../../reference/generated/skrub.choose_fromhtml.md#skrub.choose_from) it is the first
option we give it. We can also obtain random choices, or choices suggested by
an Optuna [`trial`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.trial.Trial.html#optuna.trial.Trial), by passing the `choose`
parameter.

To get a pipeline that runs an internal cross-validation to select the best
hyperparameters, we must use [`.skb.make_grid_search()`](../../../reference/generated/skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) or [`.skb.make_randomized_search()`](../../../reference/generated/skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search). We can also use [Optuna](https://optuna.readthedocs.io/) to choose the best hyperparameters as shown
in [this example](../../../auto_examples/02_data_ops/1131_optuna_choiceshtml.md#example-optuna-choices).

Here are the different kinds of choices, along with their default outcome when
we are not using hyperparameter search:

<a id="choice-defaults-table"></a>

#### Default choice outcomes

| Choosing function                                                                                                            | Description                                                                                                                               | Default outcome                                                                                             |
|------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| [`choose_from([10, 20])`](../../../reference/generated/skrub.choose_fromhtml.md#skrub.choose_from)                           | Choose between the listed options (10 and 20).                                                                                            | First outcome in the list: `10`                                                                             |
| [`choose_from({"a_name": 10, "b_name": 20})`](../../../reference/generated/skrub.choose_fromhtml.md#skrub.choose_from)       | Choose between the listed options (10 and 20). Dictionary keys serve as<br/>names for the options.                                        | First outcome in the dictionary: `10`                                                                       |
| [`optional(10)`](../../../reference/generated/skrub.optionalhtml.md#skrub.optional)                                          | Choose between the provided value and `None` (useful for optional<br/>transformations in a pipeline, e.g., `optional(StandardScaler())`). | The provided `value`: `10`                                                                                  |
| [`choose_bool()`](../../../reference/generated/skrub.choose_boolhtml.md#skrub.choose_bool)                                   | Choose between True and False.                                                                                                            | `True`                                                                                                      |
| [`choose_float(1.0, 100.0)`](../../../reference/generated/skrub.choose_floathtml.md#skrub.choose_float)                      | Sample a floating-point number in a range.                                                                                                | The middle of the range: `50.5`                                                                             |
| [`choose_int(1, 100)`](../../../reference/generated/skrub.choose_inthtml.md#skrub.choose_int)                                | Sample an integer in a range.                                                                                                             | The integer closest to the middle of the range: `50`                                                        |
| [`choose_float(1.0, 100.0, log=True)`](../../../reference/generated/skrub.choose_floathtml.md#skrub.choose_float)            | Sample a float in a range on a logarithmic scale.                                                                                         | The middle of the range on a log scale: `10.0`                                                              |
| [`choose_int(1, 100, log=True)`](../../../reference/generated/skrub.choose_inthtml.md#skrub.choose_int)                      | Sample an integer in a range on a logarithmic scale.                                                                                      | The integer closest to the middle of the range on a log scale: `10`                                         |
| [`choose_float(1.0, 100.0, n_steps=4)`](../../../reference/generated/skrub.choose_floathtml.md#skrub.choose_float)           | Sample a float on a grid.                                                                                                                 | The step closest to the middle of the range: `34.0` (steps: `[1.0, 34.0, 67.0, 100.0]`)                     |
| [`choose_int(1, 100, n_steps=4)`](../../../reference/generated/skrub.choose_inthtml.md#skrub.choose_int)                     | Sample an integer on a grid.                                                                                                              | The step closest to the middle of the range: `34` (steps: `[1, 34, 67, 100]`)                               |
| [`choose_float(1.0, 100.0, log=True, n_steps=4)`](../../../reference/generated/skrub.choose_floathtml.md#skrub.choose_float) | Sample a float on a logarithmically spaced grid.                                                                                          | The step closest to the middle of the range on a log scale: `4.64`<br/>(steps: `[1.0, 4.64, 21.54, 100.0]`) |
| [`choose_int(1, 100, log=True, n_steps=4)`](../../../reference/generated/skrub.choose_inthtml.md#skrub.choose_int)           | Sample an integer on a logarithmically spaced grid.                                                                                       | The step closest to the middle of the range on a log scale: `5`<br/>(steps: `[1, 5, 22, 100]`)              |

The default choices for a DataOp, those that get used when calling
[`.skb.make_learner()`](../../../reference/generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner), can be inspected with
[`.skb.describe_defaults()`](../../../reference/generated/skrub.DataOp.skb.describe_defaultshtml.md#skrub.DataOp.skb.describe_defaults):

```pycon
>>> pred.skb.describe_defaults()
{'α': 0.00316...}
```

We can then find the best hyperparameters.

```pycon
>>> search = pred.skb.make_randomized_search(fitted=True)
>>> search.results_
          α  mean_test_score
0  0.000480         0.482327
1  0.000287         0.482327
2  0.000014         0.482317
3  0.000012         0.482317
4  0.000006         0.482317
5  0.134157         0.478651
6  0.249613         0.472019
7  0.612327         0.442312
8  2.664713         0.308492
9  3.457901         0.275007
```

A human-readable description of parameters for a pipeline can be obtained with
[`SkrubLearner.describe_params()`](../../../reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.describe_params):

```pycon
>>> search.best_learner_.describe_params()
{'α': 0.000479...}
```

It is also possible to use [`ParamSearch.plot_results()`](../../../reference/generated/skrub.ParamSearchhtml.md#skrub.ParamSearch.plot_results) to visualize the results
of the search using a parallel coordinates plot.

This could also be done with Optuna, either by passing `backend='optuna'` to
[`DataOp.skb.make_randomized_search()`](../../../reference/generated/skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search), or by using Optuna directly:

```pycon
>>> import optuna
>>> def objective(trial):
...     learner = pred.skb.make_learner(choose=trial)
...     cv_results = skrub.cross_validate(learner, pred.skb.get_data())
...     return cv_results['test_score'].mean()
>>> study = optuna.create_study(direction="maximize")
>>> study.optimize(objective, n_trials=10)
>>> best_learner = pred.skb.make_learner(choose=study.best_trial)
>>> best_learner.describe_params()
{'α': 0.0006391165935023005}
```

Rather than fitting a randomized or grid search to find the best combination, it
is also possible to obtain an iterator over different parameter combinations to
inspect their outputs or to have manual control over the model selection. This can
be done with [`.skb.iter_learners_grid()`](../../../reference/generated/skrub.DataOp.skb.iter_learners_gridhtml.md#skrub.DataOp.skb.iter_learners_grid) or
[`.skb.iter_learners_randomized()`](../../../reference/generated/skrub.DataOp.skb.iter_learners_randomizedhtml.md#skrub.DataOp.skb.iter_learners_randomized) (
which yield the candidate pipelines that are explored by the grid and randomized
search respectively), or with the `choose` parameter of
[`.skb.make_learner()`](../../../reference/generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner).

A full example of how to use hyperparameter search is available in
[Hyperparameter tuning with DataOps](../../../auto_examples/02_data_ops/1130_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py), and a full example using
Optuna is in [Tuning DataOps with Optuna](../../../auto_examples/02_data_ops/1131_optuna_choiceshtml.md#example-optuna-choices).

<br/>

<a id="user-guide-data-ops-feature-selection"></a>

# Feature selection with skrub [`SelectCols`](../../../reference/generated/skrub.SelectColshtml.md#skrub.SelectCols) and [`DropCols`](../../../reference/generated/skrub.DropColshtml.md#skrub.DropCols)

It is possible to combine [`SelectCols`](../../../reference/generated/skrub.SelectColshtml.md#skrub.SelectCols) and [`DropCols`](../../../reference/generated/skrub.DropColshtml.md#skrub.DropCols) with
[`choose_from()`](../../../reference/generated/skrub.choose_fromhtml.md#skrub.choose_from) to perform feature selection by dropping specific columns
and evaluating how this affects the downstream performance.

Consider this example. We first define the variable:

```pycon
>>> import pandas as pd
>>> import skrub.selectors as s
>>> from sklearn.preprocessing import StandardScaler, OneHotEncoder
>>> df = pd.DataFrame({"text": ["foo", "bar", "baz"], "number": [1, 2, 3]})
>>> X = skrub.X(df)
```

Then, we use the [skrub selectors](../../multi_column_operations/selectorshtml.md#user-guide-selectors) to encode each
column with a different transformer:

```pycon
>>> X_enc = X.skb.apply(StandardScaler(), cols=s.numeric()).skb.apply(
...     OneHotEncoder(sparse_output=False), cols=s.string()
... )
>>> X_enc
<Apply OneHotEncoder>
Result:
―――――――
     number  text_bar  text_baz  text_foo
0 -1.224745       0.0       0.0       1.0
1  0.000000       1.0       0.0       0.0
2  1.224745       0.0       1.0       0.0
```

Now we can use [`skrub.DropCols`](../../../reference/generated/skrub.DropColshtml.md#skrub.DropCols) to define two possible selection strategies:
first, we drop the column `number`, then we drop all columns that start with
`text`. We rely again on the skrub selectors for this:

```pycon
>>> from skrub import DropCols
>>> drop = DropCols(cols=skrub.choose_from(
...     {"number": s.cols("number"), "text": s.glob("text_*")})
... )
>>> X_enc.skb.apply(drop)
<Apply DropCols>
Result:
―――――――
   text_bar  text_baz  text_foo
0       0.0       0.0       1.0
1       1.0       0.0       0.0
2       0.0       1.0       0.0
```

We can see the generated parameter grid with `DataOps.skb.describe_param_grid()`.

```pycon
>>> X_enc.skb.apply(drop).skb.describe_param_grid()
"- choose_from({'number': …, 'text': …}): ['number', 'text']\n"
```

A more advanced application of this technique is used in
[this tutorial on forecasting timeseries](https://skrub-data.org/EuroSciPy2025/content/notebooks/single_horizon_prediction.html),
along with the feature engineering required to prepare the columns, and the
analysis of the results.
