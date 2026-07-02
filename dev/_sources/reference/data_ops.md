<a id="data-ops-ref"></a>

# DataOps

Generalizing the scikit-learn pipeline. See [skrub DataOps](../data_opshtml.md#user-guide-data-ops-index) for further details.

| [`var`](generated/skrub.varhtml.md#skrub.var)                      | Create a skrub variable.                                                                                  |
|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| [`X`](generated/skrub.Xhtml.md#skrub.X)                            | Create a skrub variable and mark it as being `X`.                                                         |
| [`y`](generated/skrub.yhtml.md#skrub.y)                            | Create a skrub variable and mark it as being `y`.                                                         |
| [`as_data_op`](generated/skrub.as_data_ophtml.md#skrub.as_data_op) | Create a DataOp [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp) that evaluates to the given value. |
| [`deferred`](generated/skrub.deferredhtml.md#skrub.deferred)       | Wrap function calls in a DataOp [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp).                   |

The DataOp object.

| [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp)   | Representation of a computation that can be used to build DataOps plans and learners.   |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------|

Inline hyperparameters selection in your DataOps plan.

| [`choose_bool`](generated/skrub.choose_boolhtml.md#skrub.choose_bool)    | A choice between `True` and `False`.                     |
|--------------------------------------------------------------------------|----------------------------------------------------------|
| [`choose_float`](generated/skrub.choose_floathtml.md#skrub.choose_float) | A choice of floating-point numbers from a numeric range. |
| [`choose_int`](generated/skrub.choose_inthtml.md#skrub.choose_int)       | A choice of integers from a numeric range.               |
| [`choose_from`](generated/skrub.choose_fromhtml.md#skrub.choose_from)    | A choice among several possible outcomes.                |
| [`optional`](generated/skrub.optionalhtml.md#skrub.optional)             | A choice between `value` and `None`.                     |

Evaluate your DataOps plan.

| [`cross_validate`](generated/skrub.cross_validatehtml.md#skrub.cross_validate)   | Cross-validate a learner built from a DataOp.                     |
|----------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [`eval_mode`](generated/skrub.eval_modehtml.md#skrub.eval_mode)                  | Return the mode in which the DataOp is currently being evaluated. |

The `skb` accessor exposes all DataOps methods and attributes.

| [`DataOp.skb.apply`](generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply)                                                          | Apply an estimator that follows the scikit-learn API to a dataframe or numpy array.   |
|-----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| [`DataOp.skb.apply_func`](generated/skrub.DataOp.skb.apply_funchtml.md#skrub.DataOp.skb.apply_func)                                           | Apply the given function.                                                             |
| [`DataOp.skb.clone`](generated/skrub.DataOp.skb.clonehtml.md#skrub.DataOp.skb.clone)                                                          | Get an independent clone of the DataOp.                                               |
| [`DataOp.skb.concat`](generated/skrub.DataOp.skb.concathtml.md#skrub.DataOp.skb.concat)                                                       | Concatenate arrays or dataframes vertically or horizontally.                          |
| [`DataOp.skb.cross_validate`](generated/skrub.DataOp.skb.cross_validatehtml.md#skrub.DataOp.skb.cross_validate)                               | Cross-validate the DataOp plan.                                                       |
| [`DataOp.skb.describe_defaults`](generated/skrub.DataOp.skb.describe_defaultshtml.md#skrub.DataOp.skb.describe_defaults)                      | Describe the hyper-parameters used by the default learner.                            |
| [`DataOp.skb.describe_param_grid`](generated/skrub.DataOp.skb.describe_param_gridhtml.md#skrub.DataOp.skb.describe_param_grid)                | Describe the hyper-parameters extracted from choices in the DataOp.                   |
| [`DataOp.skb.describe_steps`](generated/skrub.DataOp.skb.describe_stepshtml.md#skrub.DataOp.skb.describe_steps)                               | Get a text representation of the computation graph.                                   |
| [`DataOp.skb.draw_graph`](generated/skrub.DataOp.skb.draw_graphhtml.md#skrub.DataOp.skb.draw_graph)                                           | Get an SVG string representing the computation graph.                                 |
| [`DataOp.skb.drop`](generated/skrub.DataOp.skb.drophtml.md#skrub.DataOp.skb.drop)                                                             | Drop some columns.                                                                    |
| [`DataOp.skb.eval`](generated/skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval)                                                             | Evaluate the DataOp.                                                                  |
| [`DataOp.skb.find`](generated/skrub.DataOp.skb.findhtml.md#skrub.DataOp.skb.find)                                                             | Find a node (DataOp or choice) in the computational graph.                            |
| [`DataOp.skb.find_X_y`](generated/skrub.DataOp.skb.find_X_yhtml.md#skrub.DataOp.skb.find_X_y)                                                 | Find the nodes that have been marked with `mark_as_X()` and `mark_as_y()`.            |
| [`DataOp.skb.freeze_after_fit`](generated/skrub.DataOp.skb.freeze_after_fithtml.md#skrub.DataOp.skb.freeze_after_fit)                         | Freeze the result during learner fitting.                                             |
| [`DataOp.skb.full_report`](generated/skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report)                                        | Generate a full report of the DataOp's evaluation.                                    |
| [`DataOp.skb.get_data`](generated/skrub.DataOp.skb.get_datahtml.md#skrub.DataOp.skb.get_data)                                                 | Collect the values of the variables contained in the DataOp.                          |
| [`DataOp.skb.get_vars`](generated/skrub.DataOp.skb.get_varshtml.md#skrub.DataOp.skb.get_vars)                                                 | Get all the variables used in the DataOp.                                             |
| [`DataOp.skb.if_else`](generated/skrub.DataOp.skb.if_elsehtml.md#skrub.DataOp.skb.if_else)                                                    | Create a conditional DataOp.                                                          |
| [`DataOp.skb.iter_cv_splits`](generated/skrub.DataOp.skb.iter_cv_splitshtml.md#skrub.DataOp.skb.iter_cv_splits)                               | Yield splits of an environment into training and testing environments.                |
| [`DataOp.skb.iter_learners_grid`](generated/skrub.DataOp.skb.iter_learners_gridhtml.md#skrub.DataOp.skb.iter_learners_grid)                   | Get learners with different parameter combinations.                                   |
| [`DataOp.skb.iter_learners_randomized`](generated/skrub.DataOp.skb.iter_learners_randomizedhtml.md#skrub.DataOp.skb.iter_learners_randomized) | Get learners with different parameter combinations.                                   |
| [`DataOp.skb.make_grid_search`](generated/skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)                         | Find the best parameters with grid search.                                            |
| [`DataOp.skb.make_learner`](generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)                                     | Get a skrub learner for this DataOp.                                                  |
| [`DataOp.skb.make_randomized_search`](generated/skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)       | Find the best parameters with randomized search.                                      |
| [`DataOp.skb.mark_as_X`](generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X)                                              | Mark this DataOp as being the `X` table.                                              |
| [`DataOp.skb.mark_as_y`](generated/skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y)                                              | Mark this DataOp as being the `y` table.                                              |
| [`DataOp.skb.match`](generated/skrub.DataOp.skb.matchhtml.md#skrub.DataOp.skb.match)                                                          | Select based on the value of a DataOp.                                                |
| [`DataOp.skb.preview`](generated/skrub.DataOp.skb.previewhtml.md#skrub.DataOp.skb.preview)                                                    | Get the value computed for previews (shown when printing the DataOp).                 |
| [`DataOp.skb.select`](generated/skrub.DataOp.skb.selecthtml.md#skrub.DataOp.skb.select)                                                       | Select a subset of columns.                                                           |
| [`DataOp.skb.set_data`](generated/skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data)                                                 | Get a new DataOp with the provided preview values set on the variables.               |
| [`DataOp.skb.set_description`](generated/skrub.DataOp.skb.set_descriptionhtml.md#skrub.DataOp.skb.set_description)                            | Give a description to this DataOp.                                                    |
| [`DataOp.skb.set_name`](generated/skrub.DataOp.skb.set_namehtml.md#skrub.DataOp.skb.set_name)                                                 | Give a name to this DataOp.                                                           |
| [`DataOp.skb.subsample`](generated/skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)                                              | Configure subsampling of a dataframe or numpy array.                                  |
| [`DataOp.skb.train_test_split`](generated/skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split)                         | Split an environment into training and testing environments.                          |
| [`DataOp.skb.with_scoring`](generated/skrub.DataOp.skb.with_scoringhtml.md#skrub.DataOp.skb.with_scoring)                                     | Attach a scoring method to this DataOp.                                               |

Accessor attributes.

| [`DataOp.skb.applied_estimator`](generated/skrub.DataOp.skb.applied_estimatorhtml.md#skrub.DataOp.skb.applied_estimator)   | Retrieve the estimator applied in the previous step, as a DataOp.                                                                     |
|----------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| [`DataOp.skb.description`](generated/skrub.DataOp.skb.descriptionhtml.md#skrub.DataOp.skb.description)                     | A user-defined description or comment about the DataOp.                                                                               |
| [`DataOp.skb.id`](generated/skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id)                                                | A unique ID for this DataOp.                                                                                                          |
| [`DataOp.skb.is_X`](generated/skrub.DataOp.skb.is_Xhtml.md#skrub.DataOp.skb.is_X)                                          | Whether this DataOp has been marked with [`skb.mark_as_X()`](generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X). |
| [`DataOp.skb.is_y`](generated/skrub.DataOp.skb.is_yhtml.md#skrub.DataOp.skb.is_y)                                          | Whether this DataOp has been marked with [`skb.mark_as_y()`](generated/skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y). |
| [`DataOp.skb.name`](generated/skrub.DataOp.skb.namehtml.md#skrub.DataOp.skb.name)                                          | A user-chosen name for the DataOp.                                                                                                    |

Objects generated by the DataOps.

| [`SkrubLearner`](generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner)                | Learner that evaluates a skrub DataOp.                            |
|-----------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [`ParamSearch`](generated/skrub.ParamSearchhtml.md#skrub.ParamSearch)                   | Learner that evaluates a skrub DataOp with hyperparameter tuning. |
| [`OptunaParamSearch`](generated/skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) | Learner that evaluates a skrub DataOp with hyperparameter tuning. |
