<a id="api-ref"></a>

# API

This is the class and function reference of skrub. Please refer to the
[full user guide](../documentationhtml.md#user-guide) for further details, as the raw specifications of
classes and functions may not be enough to give full guidelines on their use.

## Building a pipeline

| [`tabular_pipeline`](generated/skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline)   | Get a simple machine-learning pipeline for tabular data.        |
|----------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| [`TableVectorizer`](generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)      | Transform a dataframe to a numeric (vectorized) representation. |
| [`ApplyToCols`](generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols)                  | Apply a transformer to selected columns in a dataframe.         |
| [`SelectCols`](generated/skrub.SelectColshtml.md#skrub.SelectCols)                     | Select a subset of a DataFrame's columns.                       |
| [`DropCols`](generated/skrub.DropColshtml.md#skrub.DropCols)                           | Drop a subset of a DataFrame's columns.                         |
| [`Drop`](generated/skrub.Drophtml.md#skrub.Drop)                                       | Drop the selected DataFrame's column unconditionally.           |

## Encoding a column

| [`StringEncoder`](generated/skrub.StringEncoderhtml.md#skrub.StringEncoder)             | Encode string columns as a numeric array using Latent Semantic Analysis (LSA).                              |
|-----------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| [`TextEncoder`](generated/skrub.TextEncoderhtml.md#skrub.TextEncoder)                   | Encode string features by applying a pretrained language model         downloaded from the HuggingFace Hub. |
| [`MinHashEncoder`](generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)          | Encode string categorical features by applying the MinHash method to n-gram     decompositions of strings.  |
| [`GapEncoder`](generated/skrub.GapEncoderhtml.md#skrub.GapEncoder)                      | Encode string columns by constructing latent topics.                                                        |
| [`SimilarityEncoder`](generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) | Encode string categories to a similarity matrix, to capture fuzziness across a few categories.              |
| [`ToCategorical`](generated/skrub.ToCategoricalhtml.md#skrub.ToCategorical)             | Convert a string column to Categorical dtype.                                                               |
| [`DatetimeEncoder`](generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder)       | Extract temporal features such as month, day of the week, … from a datetime column.                         |
| [`SessionEncoder`](generated/skrub.SessionEncoderhtml.md#skrub.SessionEncoder)          | Add a session ID column to a dataframe based on time gaps and other columns.                                |
| [`ToDatetime`](generated/skrub.ToDatetimehtml.md#skrub.ToDatetime)                      | Parse datetimes represented as strings and return `Datetime` columns.                                       |
| [`ToFloat`](generated/skrub.ToFloathtml.md#skrub.ToFloat)                               | Convert a column to 32-bit floating-point numbers.                                                          |

## Exploring a dataframe

| [`TableReport`](generated/skrub.TableReporthtml.md#skrub.TableReport)                         | Summarize the contents of a dataframe.                                 |
|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| [`patch_display`](generated/skrub.patch_displayhtml.md#skrub.patch_display)                   | Replace the default DataFrame HTML displays with `skrub.TableReport`.  |
| [`unpatch_display`](generated/skrub.unpatch_displayhtml.md#skrub.unpatch_display)             | Undo the effect of `skrub.patch_display()`.                            |
| [`column_associations`](generated/skrub.column_associationshtml.md#skrub.column_associations) | Get measures of statistical associations between all pairs of columns. |

## Cleaning a dataframe

| [`SquashingScaler`](generated/skrub.SquashingScalerhtml.md#skrub.SquashingScaler)       | Perform robust centering and scaling followed by soft clipping.                                                |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [`Cleaner`](generated/skrub.Cleanerhtml.md#skrub.Cleaner)                               | Column-wise consistency checks and sanitization of dtypes, null values and dates.                              |
| [`DropUninformative`](generated/skrub.DropUninformativehtml.md#skrub.DropUninformative) | Drop column if it is found to be uninformative according to various criteria.                                  |
| [`DropSimilar`](generated/skrub.DropSimilarhtml.md#skrub.DropSimilar)                   | Drop columns found too redundant to the rest of the dataframe, according to association defined by Cramér's V. |
| [`DurationToFloat`](generated/skrub.DurationToFloathtml.md#skrub.DurationToFloat)       | Convert duration columns to seconds.                                                                           |
| [`to_datetime`](generated/skrub.to_datetimehtml.md#skrub.to_datetime)                   | Convert a dataframe or series to Datetime dtype.                                                               |
| [`deduplicate`](generated/skrub.deduplicatehtml.md#skrub.deduplicate)                   | Deduplicate categorical data by hierarchically clustering similar strings.                                     |

## Selectors

| [`selectors.Selector`](generated/skrub.selectors.Selectorhtml.md#skrub.selectors.Selector)                            | Pattern for matching columns in a dataframe.                                                                     |
|-----------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| [`selectors.all`](generated/skrub.selectors.allhtml.md#skrub.selectors.all)                                           | Select all columns in a dataframe.                                                                               |
| [`selectors.any_date`](generated/skrub.selectors.any_datehtml.md#skrub.selectors.any_date)                            | Select columns that have a Date or Datetime data type.                                                           |
| [`selectors.boolean`](generated/skrub.selectors.booleanhtml.md#skrub.selectors.boolean)                               | Select columns that have a Boolean data type.                                                                    |
| [`selectors.cardinality_below`](generated/skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below) | Select columns whose cardinality (number of unique values) is (strictly)     below `threshold`.                  |
| [`selectors.categorical`](generated/skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)                   | Select columns that have a Categorical (or polars Enum) data type.                                               |
| [`selectors.cols`](generated/skrub.selectors.colshtml.md#skrub.selectors.cols)                                        | Select columns by name.                                                                                          |
| [`selectors.drop`](generated/skrub.selectors.drophtml.md#skrub.selectors.drop)                                        | Select the columns of a dataframe that are NOT matched by the selector.                                          |
| [`selectors.filter`](generated/skrub.selectors.filterhtml.md#skrub.selectors.filter)                                  | Select columns for which a custom predicate function returns `True`.                                             |
| [`selectors.filter_names`](generated/skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names)                | Select columns based only on their name.                                                                         |
| [`selectors.float`](generated/skrub.selectors.floathtml.md#skrub.selectors.float)                                     | Select columns that have a floating-point data type (float32, float64, etc.)                                     |
| [`selectors.glob`](generated/skrub.selectors.globhtml.md#skrub.selectors.glob)                                        | Select columns by name with Unix shell style 'glob' pattern.                                                     |
| [`selectors.has_dtype`](generated/skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype)                         | Select columns whose dtype is equal to one of the provided dtypes.                                               |
| [`selectors.has_nulls`](generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls)                         | Select columns that contain at least one null value, or a proportion of null     values above a given threshold. |
| [`selectors.integer`](generated/skrub.selectors.integerhtml.md#skrub.selectors.integer)                               | Select columns that have an integer data type.                                                                   |
| [`selectors.inv`](generated/skrub.selectors.invhtml.md#skrub.selectors.inv)                                           | Invert a selector.                                                                                               |
| [`selectors.make_selector`](generated/skrub.selectors.make_selectorhtml.md#skrub.selectors.make_selector)             | Normalize a selector, column name, or list of names into a `Selector`    object.                                 |
| [`selectors.numeric`](generated/skrub.selectors.numerichtml.md#skrub.selectors.numeric)                               | Select columns that have a numeric data type.                                                                    |
| [`selectors.object`](generated/skrub.selectors.objecthtml.md#skrub.selectors.object)                                  | Select columns whose dtype is `object` (pandas) or `pl.Object` (polars).                                         |
| [`selectors.regex`](generated/skrub.selectors.regexhtml.md#skrub.selectors.regex)                                     | Select columns by name with a regular expression.                                                                |
| [`selectors.select`](generated/skrub.selectors.selecthtml.md#skrub.selectors.select)                                  | Select the columns of a dataframe that are matched by the selector.                                              |
| [`selectors.string`](generated/skrub.selectors.stringhtml.md#skrub.selectors.string)                                  | Select columns that have a string data type.                                                                     |

## DataOps

| [`var`](generated/skrub.varhtml.md#skrub.var)                      | Create a skrub variable.                                                                                  |
|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| [`X`](generated/skrub.Xhtml.md#skrub.X)                            | Create a skrub variable and mark it as being `X`.                                                         |
| [`y`](generated/skrub.yhtml.md#skrub.y)                            | Create a skrub variable and mark it as being `y`.                                                         |
| [`as_data_op`](generated/skrub.as_data_ophtml.md#skrub.as_data_op) | Create a DataOp [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp) that evaluates to the given value. |
| [`deferred`](generated/skrub.deferredhtml.md#skrub.deferred)       | Wrap function calls in a DataOp [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp).                   |

| [`DataOp`](generated/skrub.DataOphtml.md#skrub.DataOp)   | Representation of a computation that can be used to build DataOps plans and learners.   |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------|

| [`choose_bool`](generated/skrub.choose_boolhtml.md#skrub.choose_bool)    | A choice between `True` and `False`.                     |
|--------------------------------------------------------------------------|----------------------------------------------------------|
| [`choose_float`](generated/skrub.choose_floathtml.md#skrub.choose_float) | A choice of floating-point numbers from a numeric range. |
| [`choose_int`](generated/skrub.choose_inthtml.md#skrub.choose_int)       | A choice of integers from a numeric range.               |
| [`choose_from`](generated/skrub.choose_fromhtml.md#skrub.choose_from)    | A choice among several possible outcomes.                |
| [`optional`](generated/skrub.optionalhtml.md#skrub.optional)             | A choice between `value` and `None`.                     |

| [`cross_validate`](generated/skrub.cross_validatehtml.md#skrub.cross_validate)   | Cross-validate a learner built from a DataOp.                     |
|----------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [`eval_mode`](generated/skrub.eval_modehtml.md#skrub.eval_mode)                  | Return the mode in which the DataOp is currently being evaluated. |

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

| [`DataOp.skb.applied_estimator`](generated/skrub.DataOp.skb.applied_estimatorhtml.md#skrub.DataOp.skb.applied_estimator)   | Retrieve the estimator applied in the previous step, as a DataOp.                                                                     |
|----------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| [`DataOp.skb.description`](generated/skrub.DataOp.skb.descriptionhtml.md#skrub.DataOp.skb.description)                     | A user-defined description or comment about the DataOp.                                                                               |
| [`DataOp.skb.id`](generated/skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id)                                                | A unique ID for this DataOp.                                                                                                          |
| [`DataOp.skb.is_X`](generated/skrub.DataOp.skb.is_Xhtml.md#skrub.DataOp.skb.is_X)                                          | Whether this DataOp has been marked with [`skb.mark_as_X()`](generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X). |
| [`DataOp.skb.is_y`](generated/skrub.DataOp.skb.is_yhtml.md#skrub.DataOp.skb.is_y)                                          | Whether this DataOp has been marked with [`skb.mark_as_y()`](generated/skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y). |
| [`DataOp.skb.name`](generated/skrub.DataOp.skb.namehtml.md#skrub.DataOp.skb.name)                                          | A user-chosen name for the DataOp.                                                                                                    |

| [`SkrubLearner`](generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner)                | Learner that evaluates a skrub DataOp.                            |
|-----------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [`ParamSearch`](generated/skrub.ParamSearchhtml.md#skrub.ParamSearch)                   | Learner that evaluates a skrub DataOp with hyperparameter tuning. |
| [`OptunaParamSearch`](generated/skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) | Learner that evaluates a skrub DataOp with hyperparameter tuning. |

## Advanced topics

| [`core.SingleColumnTransformer`](generated/skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer)   | Base class for single-column transformers.                                    |
|----------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| [`core.RejectColumn`](generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn)                                    | Used by single-column transformers to indicate they do not apply to a column. |

## Configuration

| [`get_config`](generated/skrub.get_confightml.md#skrub.get_config)             | Retrieve current values for configuration set by [`set_config()`](generated/skrub.set_confightml.md#skrub.set_config).   |
|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| [`set_config`](generated/skrub.set_confightml.md#skrub.set_config)             | Set global skrub configuration.                                                                                          |
| [`config_context`](generated/skrub.config_contexthtml.md#skrub.config_context) | Context manager for global skrub configuration.                                                                          |

## Joining dataframes

| [`Joiner`](generated/skrub.Joinerhtml.md#skrub.Joiner)                                        | Augment features in a main table by fuzzy-joining an auxiliary table to it.                                    |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [`AggJoiner`](generated/skrub.AggJoinerhtml.md#skrub.AggJoiner)                               | Aggregate an auxiliary dataframe before joining it on a base dataframe.                                        |
| [`MultiAggJoiner`](generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner)                | Extension of the [`AggJoiner`](generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) to multiple auxiliary tables. |
| [`AggTarget`](generated/skrub.AggTargethtml.md#skrub.AggTarget)                               | Aggregate a target `y` before joining its aggregation on a base dataframe.                                     |
| [`InterpolationJoiner`](generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) | Join with a table augmented by machine-learning predictions.                                                   |
| [`fuzzy_join`](generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join)                            | Perform a fuzzy (approximate) join between the left and right tables.                                          |

## Datasets

| [`datasets.fetch_bike_sharing`](generated/skrub.datasets.fetch_bike_sharinghtml.md#skrub.datasets.fetch_bike_sharing)                                  | Fetch the bike sharing dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)              |
|--------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`datasets.fetch_california_housing`](generated/skrub.datasets.fetch_california_housinghtml.md#skrub.datasets.fetch_california_housing)                | Fetches the california housing dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)     |
| [`datasets.fetch_country_happiness`](generated/skrub.datasets.fetch_country_happinesshtml.md#skrub.datasets.fetch_country_happiness)                   | Fetch the happiness index dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)           |
| [`datasets.fetch_credit_fraud`](generated/skrub.datasets.fetch_credit_fraudhtml.md#skrub.datasets.fetch_credit_fraud)                                  | Fetch the credit fraud dataset (classification).                                                                                                                               |
| [`datasets.fetch_drug_directory`](generated/skrub.datasets.fetch_drug_directoryhtml.md#skrub.datasets.fetch_drug_directory)                            | Fetches the drug directory dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)     |
| [`datasets.fetch_electricity_forecasting`](generated/skrub.datasets.fetch_electricity_forecastinghtml.md#skrub.datasets.fetch_electricity_forecasting) | Fetches the electricity usage dataset (forecasting), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)     |
| [`datasets.fetch_employee_salaries`](generated/skrub.datasets.fetch_employee_salarieshtml.md#skrub.datasets.fetch_employee_salaries)                   | Fetches the employee salaries dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)      |
| [`datasets.fetch_flight_delays`](generated/skrub.datasets.fetch_flight_delayshtml.md#skrub.datasets.fetch_flight_delays)                               | Fetch the flight delays dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)             |
| [`datasets.fetch_medical_charge`](generated/skrub.datasets.fetch_medical_chargehtml.md#skrub.datasets.fetch_medical_charge)                            | Fetches the medical charge dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)         |
| [`datasets.fetch_midwest_survey`](generated/skrub.datasets.fetch_midwest_surveyhtml.md#skrub.datasets.fetch_midwest_survey)                            | Fetches the midwest survey dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)     |
| [`datasets.fetch_movielens`](generated/skrub.datasets.fetch_movielenshtml.md#skrub.datasets.fetch_movielens)                                           | Fetch the movielens dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)                 |
| [`datasets.fetch_open_payments`](generated/skrub.datasets.fetch_open_paymentshtml.md#skrub.datasets.fetch_open_payments)                               | Fetches the open payments dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)      |
| [`datasets.fetch_toxicity`](generated/skrub.datasets.fetch_toxicityhtml.md#skrub.datasets.fetch_toxicity)                                              | Fetch the toxicity dataset (classification) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)              |
| [`datasets.fetch_traffic_violations`](generated/skrub.datasets.fetch_traffic_violationshtml.md#skrub.datasets.fetch_traffic_violations)                | Fetches the traffic violations dataset (classification), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files) |
| [`datasets.fetch_videogame_sales`](generated/skrub.datasets.fetch_videogame_saleshtml.md#skrub.datasets.fetch_videogame_sales)                         | Fetch the videogame sales dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)           |
| [`datasets.get_data_dir`](generated/skrub.datasets.get_data_dirhtml.md#skrub.datasets.get_data_dir)                                                    | Returns the directory in which skrub looks for data.                                                                                                                           |
| [`datasets.make_deduplication_data`](generated/skrub.datasets.make_deduplication_datahtml.md#skrub.datasets.make_deduplication_data)                   | Duplicates examples with spelling mistakes.                                                                                                                                    |
| [`datasets.toy_orders`](generated/skrub.datasets.toy_ordershtml.md#skrub.datasets.toy_orders)                                                          | Create a toy dataframe and corresponding targets for examples.                                                                                                                 |
| [`datasets.toy_products`](generated/skrub.datasets.toy_productshtml.md#skrub.datasets.toy_products)                                                    |                                                                                                                                                                                |
| [`datasets.toy_cities`](generated/skrub.datasets.toy_citieshtml.md#skrub.datasets.toy_cities)                                                          | Generate a synthetic dataframe example with a variety of column types.                                                                                                         |
| [`datasets.make_retail_events`](generated/skrub.datasets.make_retail_eventshtml.md#skrub.datasets.make_retail_events)                                  | Generate a synthetic e-commerce clickstream dataset for classification.                                                                                                        |
