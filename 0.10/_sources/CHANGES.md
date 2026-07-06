<a id="changes"></a>

# Release history

## Release 0.10.0

### New Features

**Transformers**:

- The [`SessionEncoder`](reference/generated/skrub.SessionEncoderhtml.md#skrub.SessionEncoder) is now available. This encoder adds a `session_id`
  column, which groups together events that occur within the given session gap.
  Additionally, it is possible to provide a `split_by` column or list of columns
  (e.g., user ID or (user ID, user device)) to compute sessions for each grouping
  value.
  [#1930](https://github.com/skrub-data/skrub/pull/1930) by  [Riccardo Cappuzzo](https://github.com/rcap107).
- The [`DropSimilar`](reference/generated/skrub.DropSimilarhtml.md#skrub.DropSimilar) transformer has been added, for removing columns that
  present high correlation with other columns in a dataframe . [#2023](https://github.com/skrub-data/skrub/pull/2023) by
  [Eloi Massoulié](https://github.com/emassoulie).
- [`ToFloat`](reference/generated/skrub.ToFloathtml.md#skrub.ToFloat) now allows users to specify `decimal` and `thousand`
  separators to parse numerical columns that use formatting different from the default
  formatting used in Python, such as `1'234,5`.
  Additionally, negative numbers indicated with parentheses can be converted to the
  regular numeric format (`(432)` becomes `-432`). [#1772](https://github.com/skrub-data/skrub/pull/1772) by [Gabriela
  Gómez Jiménez](https://github.com/gabrielapgomezji).
- The transformer [`DurationToFloat`](reference/generated/skrub.DurationToFloathtml.md#skrub.DurationToFloat) has been added. [#2069](https://github.com/skrub-data/skrub/pull/2069) by
  [Riccardo Cappuzzo](https://github.com/rcap107).

**TableReport**:

- [`TableReport.json()`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport.json) now includes histogram data for numeric and datetime
  columns (the bin count and edges, and numbers of low and high outliers). Now
  `json()` contains all the information shown in the report HTML rendering,
  including the plots. The schema of the generated JSON is available at
  [TableReport JSON schema](reference/table_report_json_schemahtml.md#table-report-json-schema).
  [#2164](https://github.com/skrub-data/skrub/pull/2164) by [Jérôme Dockès](https://github.com/jeromedockes).
- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) can now be exported in markdown format with
  [`markdown()`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport.markdown).
  [#2048](https://github.com/skrub-data/skrub/pull/2048) by [Riccardo Cappuzzo](https://github.com/rcap107).
- [`TableReport.dict()`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport.dict) now allows exporting the report data as a Python
  dictionary. [#2188](https://github.com/skrub-data/skrub/pull/2188) by [m4nn2609-dot](https://github.com/m4nn2609-dot).

**Data Ops**:

- [`ParamSearch.plot_results()`](reference/generated/skrub.ParamSearchhtml.md#skrub.ParamSearch.plot_results) and [`OptunaParamSearch.plot_results()`](reference/generated/skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch.plot_results)
  accept new parameters `show_scores`, `show_choices`, and `show_times` to
  control respectively which scores, choices (params), and times (fit or score
  durations) should be included in the figure. [#2202](https://github.com/skrub-data/skrub/pull/2202) by [Jérôme
  Dockès](https://github.com/jeromedockes).
- New [`SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) methods
  [`SkrubLearner.get_named_params()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.get_named_params) and
  [`SkrubLearner.set_named_params()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.set_named_params) allow getting and
  setting the outcomes for
  choices contained in the DataOp, keyed by choice name. They provide a more
  robust way of transferring selected hyperparameters from one DataOp to a
  different one than [`SkrubLearner.get_params()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.get_params) and
  [`SkrubLearner.set_params()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.set_params).
  [#2090](https://github.com/skrub-data/skrub/pull/2090) by [Jérôme Dockès](https://github.com/jeromedockes).
- A parameter `becomes_default` has been added to [`var()`](reference/generated/skrub.varhtml.md#skrub.var). It allows
  indicating that the provided preview `value` should also be treated as a
  default value for this variable in all contexts (for example in a
  SkrubLearner’s method like `fit` or `predict`).
  [#2082](https://github.com/skrub-data/skrub/pull/2082) by [Jérôme Dockès](https://github.com/jeromedockes).
- It is now possible to attach new preview values to the variables in a DataOp
  with [`DataOp.skb.set_data()`](reference/generated/skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data). [#2081](https://github.com/skrub-data/skrub/pull/2081) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) objects have a new attribute [`DataOp.skb.id`](reference/generated/skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id).
  [`DataOp.skb.id`](reference/generated/skrub.DataOp.skb.idhtml.md#skrub.DataOp.skb.id)
  provides an alternative for referring to a node in the environment passed to
  [`DataOp.skb.eval()`](reference/generated/skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval), `SkrubLearner.predict()`, etc., or in
  [`DataOp.skb.find()`](reference/generated/skrub.DataOp.skb.findhtml.md#skrub.DataOp.skb.find) or [`SkrubLearner.truncated_after()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.truncated_after). [#2062](https://github.com/skrub-data/skrub/pull/2062) by
  [Jérôme Dockès](https://github.com/jeromedockes).

**Misc**:

- A new synthetic dataset generator for timestamped data and session-based
  operations has been added: [`make_retail_events()`](reference/generated/skrub.datasets.make_retail_eventshtml.md#skrub.datasets.make_retail_events).
  [#1930](https://github.com/skrub-data/skrub/pull/1930) by  [Riccardo Cappuzzo](https://github.com/rcap107).
- Added the [`object()`](reference/generated/skrub.selectors.objecthtml.md#skrub.selectors.object) selector to select columns with the
  `object` (pandas) or `pl.Object` (polars) dtype. [#2171](https://github.com/skrub-data/skrub/pull/2171) by
  [Omkar Kabde](https://github.com/omkar-334).
- [`set_config()`](reference/generated/skrub.set_confightml.md#skrub.set_config) and [`config_context()`](reference/generated/skrub.config_contexthtml.md#skrub.config_context) now accept a
  `table_report_n_rows` parameter to globally control the default number of
  rows displayed in [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport).
  [#2193](https://github.com/skrub-data/skrub/pull/2193) by [Mann](https://github.com/m4nn2609-dot).

### Changes

- [`SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner)’s `SkrubLearner.score()`
  has been enhanced when the DataOp used
  [`DataOp.skb.with_scoring()`](reference/generated/skrub.DataOp.skb.with_scoringhtml.md#skrub.DataOp.skb.with_scoring). During scoring, predict(), predict_proba()
  etc. are cached to avoid recomputation when multiple scorers are used (or one
  scorer calls them several times). Moreover it is possible to pass
  `return_predictions=True` to also retrieve any predictions that have been
  computed during scoring, in addition to the scores. Finally, in cases where we
  already have the predictions but want the result of score() without
  recomputing them, it is possible to provide them in the environment passed to
  `score({..., "_skrub_predictions": {"predict_proba": ...}})`.
  [#2195](https://github.com/skrub-data/skrub/pull/2195) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`SkrubLearner.find_fitted_estimator()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.find_fitted_estimator) now supports searching for the
  apply node by ID or callable predicate as alternatives to the node name.
  [#2194](https://github.com/skrub-data/skrub/pull/2194) by [Jérôme Dockès](https://github.com/jeromedockes).
- The minimum required version of matplotlib has been increased from 3.4.3 to 3.6.1.
  [#2159](https://github.com/skrub-data/skrub/pull/2159) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Gallery examples have been grouped into subject-specific sections. [#2102](https://github.com/skrub-data/skrub/pull/2102) by
  [Maureen Githaiga](https://github.com/maureen-githaiga).
- [`choose_from()`](reference/generated/skrub.choose_fromhtml.md#skrub.choose_from) now transparently converts `outcomes` to a list when it is
  another type of sequence. [#2100](https://github.com/skrub-data/skrub/pull/2100) by [aidbar](https://github.com/aidbar).
- An unnecessary warning that was raised when passing a numpy array to the
  TableVectorizer has been removed. [#1908](https://github.com/skrub-data/skrub/pull/1908) by
  [Sandrine Henry](https://github.com/sandrineh).
- The error message displayed in the Associations tab of the [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport)
  has been improved for reports where only one column is present.
  [#2094](https://github.com/skrub-data/skrub/pull/2094) by [Alicja Kosak](https://github.com/AlicjaKo).
- Added support for numpy arrays in [`DataOp.skb.concat()`](reference/generated/skrub.DataOp.skb.concathtml.md#skrub.DataOp.skb.concat).
  [#2096](https://github.com/skrub-data/skrub/pull/2096) by [Ayesha Siddiqua](https://github.com/siddiqua-tamk).

### Bugfixes

- A bug in how the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) treated columns
  duration columns in pandas and polars has been fixed. Now, both classes convert
  durations to the total number of seconds (with fractional part). This is done
  by the new transformer [`DurationToFloat`](reference/generated/skrub.DurationToFloathtml.md#skrub.DurationToFloat). [#2069](https://github.com/skrub-data/skrub/pull/2069) by
  [Riccardo Cappuzzo](https://github.com/rcap107).
- An error that could arise when running [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) on dataframes containing
  double dollar (`$$`) signs has been fixed.
  [#2154](https://github.com/skrub-data/skrub/pull/2154) by [Katerina Michenina](https://github.com/Michenina-Lab),
  [CecilyTS](https://github.com/CecilyTS), [Eve Rabin](https://github.com/eve2705).
- [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) with the default `hashing="fast"` now uses every
  n-gram size in `ngram_range` (the upper bound is inclusive, as documented
  and as already done by `hashing="murmur"`). Previously the largest size was
  dropped, so the default `ngram_range=(2, 4)` ignored 4-grams and a
  single-size range such as `(3, 3)` produced the same constant encoding for
  every string. [#2168](https://github.com/skrub-data/skrub/pull/2168) by [José Maia](https://github.com/glitch-ux).
- An error that happened when running [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) or `column_associations`
  on some dataframes with non-string column names has been fixed in [#2179](https://github.com/skrub-data/skrub/pull/2179)
  by [Jérôme Dockès](https://github.com/jeromedockes).
- An error that could arise in histograms when running [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) on
  data with a very small range (less than 10 representable floating-point
  numbers between min and max) has been fixed.
  [#2189](https://github.com/skrub-data/skrub/pull/2189) by [Jérôme Dockès](https://github.com/jeromedockes).

### Deprecations

- The parameter `order_by` of [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) is deprecated. Passing
  `order_by` now emits a [`DeprecationWarning`](https://docs.python.org/3/library/exceptions.html#DeprecationWarning)
  [#2101](https://github.com/skrub-data/skrub/pull/2101) by [Heidi Koivisto](https://github.com/uniheko).

## Release 0.9.0

### New Features

- It is now possible to pass additional (dynamically computed) arguments to the
  scorers used by [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) objects for validation, hyperparameter search
  etc. For example, sample weights. This is achieved by passing the scorers and
  their arguments to [`DataOp.skb.with_scoring()`](reference/generated/skrub.DataOp.skb.with_scoringhtml.md#skrub.DataOp.skb.with_scoring). [#1995](https://github.com/skrub-data/skrub/pull/1995) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- The diagrams displayed in notebooks for [`SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner),
  [`ParamSearch`](reference/generated/skrub.ParamSearchhtml.md#skrub.ParamSearch) and [`OptunaParamSearch`](reference/generated/skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) have been improved and now
  display the [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) they contain. [#2024](https://github.com/skrub-data/skrub/pull/2024) by [Jérôme Dockès](https://github.com/jeromedockes).
- The method [`DataOp.skb.find()`](reference/generated/skrub.DataOp.skb.findhtml.md#skrub.DataOp.skb.find) can find a node by name (or by a callable
  predicate) in a DataOp. The method [`DataOp.skb.find_X_y()`](reference/generated/skrub.DataOp.skb.find_X_yhtml.md#skrub.DataOp.skb.find_X_y) finds the nodes
  marked with [`DataOp.skb.mark_as_X()`](reference/generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X) and [`DataOp.skb.mark_as_y()`](reference/generated/skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y), and
  the `cv` splitter and `split_kwargs` passed to
  [`DataOp.skb.mark_as_X()`](reference/generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X), if they exist. [#2041](https://github.com/skrub-data/skrub/pull/2041)
  by [Jérôme Dockès](https://github.com/jeromedockes).
- [`selectors.has_dtype()`](reference/generated/skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype) has been added, allowing users to select columns
  by passing the dtype objects they want to match. [#2027](https://github.com/skrub-data/skrub/pull/2027) by
  [kudos07](https://github.com/kudos07).
- A new dataframe generator, [`datasets.toy_cities()`](reference/generated/skrub.datasets.toy_citieshtml.md#skrub.datasets.toy_cities), has been added for
  use cases on dataframes with variable sizes and variable correlation between
  columns. [#2042](https://github.com/skrub-data/skrub/pull/2042) by [Eloi Massoulié](https://github.com/emassoulie).
- A new selector function, `selectors.drop()`, has been added to drop columns
  from a dataframe using a selector. It mirrors the behavior of [`selectors.select()`](reference/generated/skrub.selectors.selecthtml.md#skrub.selectors.select).
  [#2108](https://github.com/skrub-data/skrub/pull/2108) by [Mary Njoroge](https://github.com/Maryahcee).

### Changes

- [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now accepts `plot_distributions` and
  `compute_associations` parameters (`True`, `False`, or `"auto"`)
  to explicitly control whether distribution plots and pairwise associations
  are computed. The threshold parameters controlling the maximum number of
  columns for which these are computed have been renamed to
  `plots_threshold` and `associations_threshold` for clarity.
  [#1907](https://github.com/skrub-data/skrub/pull/1907) by [JulietteBgl](https://github.com/JulietteBgl).
- The row indices of training and testing samples are now also included in the
  dictionaries produced by [`DataOp.skb.iter_cv_splits()`](reference/generated/skrub.DataOp.skb.iter_cv_splitshtml.md#skrub.DataOp.skb.iter_cv_splits). [#2012](https://github.com/skrub-data/skrub/pull/2012) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- The [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) now exposes a `parse_numbers` boolean parameter to
  control whether numeric-looking strings (e.g., `["1", "2", "3"]`) are parsed
  to `float32`, and a `cast_to_float` parameter to downcast numeric
  columns to `float32`.
  [#1910](https://github.com/skrub-data/skrub/pull/1910) by [Varshith-yadaV](https://github.com/Varshith-yadaV).
- [`fetch_toxicity()`](reference/generated/skrub.datasets.fetch_toxicityhtml.md#skrub.datasets.fetch_toxicity) now returns a shuffled version of the dataset by default.
  [#1892](https://github.com/skrub-data/skrub/pull/1892) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Added a `metric` parameter to [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) and [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) to configure
  the nearest-neighbor distance used for matching. The metric can be any value
  supported by [`NearestNeighbors`](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.NearestNeighbors.html#sklearn.neighbors.NearestNeighbors) (see its docstring).
  [#1861](https://github.com/skrub-data/skrub/pull/1861) by [Saba Siddique](https://github.com/sabasiddique1).
- [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) now accepts an `exclude_cols` parameter, making it
  possible to transform the columns selected by `cols` except for an
  explicit subset, mirroring [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply).
  [#2039](https://github.com/skrub-data/skrub/pull/2039) by [Saba Siddique](https://github.com/sabasiddique1).
- In python versions >= 3.11, [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) now produces better error
  tracebacks when the wrapped transformer fails, . [#1979](https://github.com/skrub-data/skrub/pull/1979) by [Jérôme
  Dockès](https://github.com/jeromedockes).
- The parameter `how` of [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) is replaced by a simpler
  Boolean parameter `no_wrap`. [#2049](https://github.com/skrub-data/skrub/pull/2049) by [Jérôme Dockès](https://github.com/jeromedockes).
- The `exclude_cols` of [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) can now be a DataOp.
  [#2050](https://github.com/skrub-data/skrub/pull/2050) by [Jérôme Dockès](https://github.com/jeromedockes).
- Skrub estimators now correctly show links to the documentation in the HTML
  representation that is generated for notebooks. [#2036](https://github.com/skrub-data/skrub/pull/2036) by [Riccardo
  Cappuzzo](https://github.com/rcap107).

### Bugfixes

- An error that could arise when calling `score` on a `SkrubLearner` that
  contains an inner transformer that has a `score` method has been fixed.
  [#2052](https://github.com/skrub-data/skrub/pull/2052) by [Jérôme Dockès](https://github.com/jeromedockes).

### Deprecations

- The parameter `numeric_dtype` in the [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) has been deprecated in
  favor of `cast_to_float` in [#1910](https://github.com/skrub-data/skrub/pull/1910).
- The parameter `drop_if_unique` of [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) and [`DropUninformative`](reference/generated/skrub.DropUninformativehtml.md#skrub.DropUninformative)
  has been deprecated. [#2040](https://github.com/skrub-data/skrub/pull/2040) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The parameters `max_plot_columns` and `max_association_columns` of the
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) have been deprecated in favor of `plot_distributions`
  and `compute_associations`. [#1907](https://github.com/skrub-data/skrub/pull/1907).

## Release 0.8.0

### New Features

- The `eager_data_ops` [configuration](guides/utilities/customizing_configurationhtml.md#user-guide-configuration-parameters) option has been added. When set to
  False, no previews are computed and validation is deferred until the DataOp is
  actually used (e.g. with `.skb.eval()`) rather than as soon as it is
  defined. This can make the definition of complex DataOps with many nodes
  faster (the overhead it removes typically becomes noticeable only in DataOps
  with 50-100 nodes or more). Moreover, the evaluation of large DataOps has also
  become faster. [#1890](https://github.com/skrub-data/skrub/pull/1890) by [Jérôme Dockès](https://github.com/jeromedockes).
- The reports produced by [`DataOp.skb.full_report()`](reference/generated/skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report) and
  [`SkrubLearner.report()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.report) now also display the values provided in the
  environment. [#1920](https://github.com/skrub-data/skrub/pull/1920) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner), [`ParamSearch`](reference/generated/skrub.ParamSearchhtml.md#skrub.ParamSearch) and [`OptunaParamSearch`](reference/generated/skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) expose
  some more attributes for inspection by scikit-learn: `__sklearn_tags__`,
  `classes_`, `_estimator_type`. [#1931](https://github.com/skrub-data/skrub/pull/1931) by [Jérôme Dockès](https://github.com/jeromedockes).
- It is now possible to pass additional (dynamically computed) arguments to the
  cross-validation splitter used by [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) objects for validation,
  hyperparameter search etc. For example, the groups for a
  [`sklearn.model_selection.GroupKFold`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GroupKFold.html#sklearn.model_selection.GroupKFold) can be computed as part of the
  DataOp evaluation and used for splitting. This is achieved by passing the
  splitter and its arguments to [`DataOp.skb.mark_as_X()`](reference/generated/skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X). [#1943](https://github.com/skrub-data/skrub/pull/1943) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- [`selectors.has_nulls()`](reference/generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls) now takes a `proportion` parameter, which allows
  selecting columns that have a fraction of null values above the given threshold.
  [#1881](https://github.com/skrub-data/skrub/pull/1881) by [Gabriela Gómez Jiménez](https://github.com/gabrielapgomezji).
- Added a new dataset, `fetch_electricity_usage()`, which contains electricity usage data
  for several French cities and corresponding weather data.
  [#2013](https://github.com/skrub-data/skrub/pull/2013) by [Lisa McBride](https://github.com/lisaleemcb).

### Changes

- Increased the minimum version of polars from 0.20 to 1.5.0.
  [#1897](https://github.com/skrub-data/skrub/pull/1897) by [Riccardo Cappuzzo](https://github.com/rcap107).
- `ApplyToCols` and `ApplyToFrame` have been merged into a single class,
  [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols),that covers the functionality of both the old classes by
  detecting automatically whether the provided transformer should be applied
  independently on each column, or on all selected columns as a single dataframe.
  As a result, `ApplyToCols` and `ApplyToFrame` have been removed.
  [#1913](https://github.com/skrub-data/skrub/pull/1913), [#1919](https://github.com/skrub-data/skrub/pull/1919) and [#1962](https://github.com/skrub-data/skrub/pull/1962) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The dataset fetcher functions now include a “path” field for each table in the dataset.
  For example, the dataset “employee_salaries” now has the field `employee_salaries_path`.
  Additionally, datasets that include a single table have the field `path`. These
  fields contain the paths to the datasets stored in the `skrub_data` folder.
  The default `skrub_data` folder can now be set in the skrub configuration and by setting
  the `SKB_DATA_DIRECTORY` environment variable. The environment variable `SKRUB_DATA_DIRECTORY`
  is deprecated and will be removed in a future version of skrub.
  [#1852](https://github.com/skrub-data/skrub/pull/1852) by [Riccardo Cappuzzo](https://github.com/rcap107). Examples in the gallery have
  been updated accordingly in [#1940](https://github.com/skrub-data/skrub/pull/1940) and [#1964](https://github.com/skrub-data/skrub/pull/1964) by [MuditAtrey](https://github.com/MuditAtrey).
- [`SingleColumnTransformer`](reference/generated/skrub.core.SingleColumnTransformerhtml.md#skrub.core.SingleColumnTransformer) and associated exception
  [`RejectColumn`](reference/generated/skrub.core.RejectColumnhtml.md#skrub.core.RejectColumn) (used internally by many skrub estimators) have
  been added to the public API, in the newly-created `skrub.core` module.
  [#1851](https://github.com/skrub-data/skrub/pull/1851) by [Eloi Massoulié](https://github.com/emassoulie).
- Added the strings `"None"` and `"none"` to the list of null string values in
  [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner). Also, exposed the list of null string values that will be set
  to null by the [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) as the parameter `null_strings`.
  [#1952](https://github.com/skrub-data/skrub/pull/1952) and [#1954](https://github.com/skrub-data/skrub/pull/1954) by [Lisa McBride](https://github.com/lisaleemcb).
- The configuration parameter “use_table_report” has been removed from the skrub
  configuration. Use [`patch_display()`](reference/generated/skrub.patch_displayhtml.md#skrub.patch_display) instead.
  [#1973](https://github.com/skrub-data/skrub/pull/1973) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Updated how the `column_filters` parameter of [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) works.
  It now accepts a dictionary where the key is the display name for the
  dropdown menu, and the value is a filter of the columns that will be displayed.
  Accepts either a list of column indices, a list of column names
  or an instance of the `Selector`.
  [#1976](https://github.com/skrub-data/skrub/pull/1976) by [Lisa McBride](https://github.com/lisaleemcb).
- The overplotting of the counts atop the vertical histogram bars in the
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) has been removed due to formatting issues.
  [#1984](https://github.com/skrub-data/skrub/pull/1984) by [Lisa McBride](https://github.com/lisaleemcb).
- The maximum number of associations that can be displayed in the
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) has been increased to N=1000, and the associations
  are now displayed in a scrollable table.
  [#1992](https://github.com/skrub-data/skrub/pull/1992) by [Lisa McBride](https://github.com/lisaleemcb).

### Bug Fixes

- The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) now correctly handles the case where one of the
  provided encoders is a scikit-learn Pipeline that starts with a skrub
  single-column transformer. [#1899](https://github.com/skrub-data/skrub/pull/1899) by [Jérôme Dockès](https://github.com/jeromedockes)
  and [#1900](https://github.com/skrub-data/skrub/pull/1900) by [Jérôme Dockès](https://github.com/jeromedockes).
- Errors raised when a polars LazyFrame is passed where an eager DataFrame is
  expected are now clearer. [#1916](https://github.com/skrub-data/skrub/pull/1916) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`DataOp.skb.cross_validate()`](reference/generated/skrub.DataOp.skb.cross_validatehtml.md#skrub.DataOp.skb.cross_validate) would raise an error when passed
  `return_indices=True`. Now it returns the train and test indices of each
  fold in the `train_indices` and `test_indices` columns of the result
  dataframe. [#1953](https://github.com/skrub-data/skrub/pull/1953) by [Jérôme Dockès](https://github.com/jeromedockes).
- Polars LazyFrames are no longer collected automatically anywhere in the library;
  a `TypeError` is now raised instead.
  [#1941](https://github.com/skrub-data/skrub/pull/1941) by [Mudit Atrey](https://github.com/MuditAtrey).

## Release 0.7.2

### Changes

- The [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) now exposes the `vocabulary` parameter from the parent
  `TfidfVectorizer`.
  [#1819](https://github.com/skrub-data/skrub/pull/1819) by [Eloi Massoulié](https://github.com/emassoulie)
- `compute_ngram_distance()` has been renamed to `_compute_ngram_distance()` and is now a private function.
  [#1838](https://github.com/skrub-data/skrub/pull/1838) by [Siddharth Baleja](https://github.com/siddharthbaleja).

### Bugfixes

- Fixed some issues related to the release of Pandas 3.0. [#1855](https://github.com/skrub-data/skrub/pull/1855) by [Riccardo Cappuzzo](https://github.com/rcap107).

## Release 0.7.1

### New features

- A new dataset, `fetch_california_housing()`, has been added to the
  `skrub.datasets` module. It allows to get a redundancy copy of the scikit-learn
  `fetch_california_housing()` function.
  [#1830](https://github.com/skrub-data/skrub/pull/1830) by [Guillaume Lemaitre](https://github.com/glemaitre).

### Bugfixes

- [`DropCols`](reference/generated/skrub.DropColshtml.md#skrub.DropCols) and `SelectCols:` attributes were renamed to end
  with an underscore, in order to follow a scikit-learn convention which is
  used to determine if an estimator is fitted. [#1813](https://github.com/skrub-data/skrub/pull/1813) by [Auguste
  Baum](https://github.com/auguste-probabl).

## Release 0.7.0

### New features

- It is now possible to tune the choices in a [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) with [Optuna](https://optuna.readthedocs.io/en/stable/). See
  [Tuning DataOps with Optuna](auto_examples/02_data_ops/1131_optuna_choiceshtml.md#example-optuna-choices) for an example.
  [#1661](https://github.com/skrub-data/skrub/pull/1661) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) now allows passing extra named arguments to the
  estimator’s methods through the parameters `fit_kwargs`, `predict_kwargs`
  etc. [#1642](https://github.com/skrub-data/skrub/pull/1642) by [Jérôme Dockès](https://github.com/jeromedockes).
- TableReport now displays the mean statistic for boolean columns.
  [#1647](https://github.com/skrub-data/skrub/pull/1647) by [Abdelhakim Benechehab](https://github.com/abenechehab).
- [`DataOp.skb.get_vars()`](reference/generated/skrub.DataOp.skb.get_varshtml.md#skrub.DataOp.skb.get_vars) allows inspecting all the variables, or all the
  named dataops, in a [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp). This lets us easily know what keys should
  be present in the `environment` dictionary we pass to
  [`DataOp.skb.eval()`](reference/generated/skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval) or to `SkrubLearner.fit()`,
  `SkrubLearner.predict()`, etc.
  [#1646](https://github.com/skrub-data/skrub/pull/1646) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`DataOp.skb.iter_cv_splits()`](reference/generated/skrub.DataOp.skb.iter_cv_splitshtml.md#skrub.DataOp.skb.iter_cv_splits) iterates over the training and testing
  environments produced by a CV splitter – similar to
  [`DataOp.skb.train_test_split()`](reference/generated/skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split) but for multiple cross-validation splits.
  [#1653](https://github.com/skrub-data/skrub/pull/1653) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now supports `np.array`. [#1676](https://github.com/skrub-data/skrub/pull/1676) by [Nisma Amjad](https://github.com/Nismamjad1).
- [`DataOp.skb.full_report()`](reference/generated/skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report) now accepts a new parameter, `title`, that is displayed
  in the html report.
  [#1654](https://github.com/skrub-data/skrub/pull/1654) by [Marie Sacksick](https://github.com/MarieSacksick).
- [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now includes the `open_tab` parameter, which lets the
  user select which tab should be opened when the `TableReport` is
  rendered. [#1737](https://github.com/skrub-data/skrub/pull/1737) by [Riccardo Cappuzzo](https://github.com/rcap107).
- [`selectors.Selector`](reference/generated/skrub.selectors.Selectorhtml.md#skrub.selectors.Selector) now has documentation for its [`selectors.Selector.expand()`](reference/generated/skrub.selectors.Selectorhtml.md#skrub.selectors.Selector.expand)
  and [`selectors.Selector.expand_index()`](reference/generated/skrub.selectors.Selectorhtml.md#skrub.selectors.Selector.expand_index) methods, with added information and examples
  in the user guide, as well as mentions in the corresponding constructor functions.
  [#1841](https://github.com/skrub-data/skrub/pull/1841) by [Eloi Massoulié](https://github.com/emassoulie).

### Changes

- The minimum supported version of Python has been increased to 3.10. Additionally,
  the minimum supported versions of scikit-learn and requests are 1.4.2 and 2.27.1
  respectively. Support for python 3.14 has been added.
  [#1572](https://github.com/skrub-data/skrub/pull/1572) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The [`DataOp.skb.full_report()`](reference/generated/skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report) method now deletes reports created with
  `output_dir=None` after 7 days. [#1657](https://github.com/skrub-data/skrub/pull/1657) by [Simon Dierickx](https://github.com/simon.dierickx).
- The [`tabular_pipeline()`](reference/generated/skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline) uses a [`SquashingScaler`](reference/generated/skrub.SquashingScalerhtml.md#skrub.SquashingScaler) instead of a
  `StandardScaler` for centering and scaling numerical features
  when linear models are used.
  [#1644](https://github.com/skrub-data/skrub/pull/1644) by [Simon Dierickx](https://github.com/dierickxsimon)
- The transformer [`ToFloat`](reference/generated/skrub.ToFloathtml.md#skrub.ToFloat), previously called `ToFloat32`, is now public.
  [#1687](https://github.com/skrub-data/skrub/pull/1687) by [Marie Sacksick](https://github.com/MarieSacksick).
- Improved the error message raised when a Polars lazyframe is passed to
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport), clarifying that `.collect()` must be called first.
  [#1767](https://github.com/skrub-data/skrub/pull/1767) by [Fatima Ben Kadour](https://github.com/fatiben2002).
- Computing the associations in [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) is now deterministic and can
  be controlled by the new parameter `subsampling_seed` of the global configuration.
  [#1775](https://github.com/skrub-data/skrub/pull/1775) by [Thomas S.](https://github.com/thomass-dev).
- Added `cast_to_str` parameter to [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) to prevent unintended
  conversion of list/object-like columns to strings unless explicitly enabled.
  [#1789](https://github.com/skrub-data/skrub/pull/1789) by [@PilliSiddharth](https://github.com/PilliSiddharth).

### Bugfixes

- The [`skrub.cross_validate()`](reference/generated/skrub.cross_validatehtml.md#skrub.cross_validate) function now raises a specific exception if the wrong variable
  type is passed.
  [#1799](https://github.com/skrub-data/skrub/pull/1799) by [Eloi Massoulié](https://github.com/emassoulie)
- Fixed various issues with some transformers by adding `get_feature_names_out`
  to all single column transformers.
  [#1666](https://github.com/skrub-data/skrub/pull/1666) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Issues occurring when [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) was passed a DataOp as the
  estimator have been fixed in [#1671](https://github.com/skrub-data/skrub/pull/1671) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) could raise an error while trying to check if Polars
  columns with some dtypes (lists, structs) are sorted. It would not indicate
  Polars columns sorted in descending order. Fixed in [#1673](https://github.com/skrub-data/skrub/pull/1673) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- Fixed nightly checks and added support for upcoming library versions, including Pandas
  v3.0. [#1664](https://github.com/skrub-data/skrub/pull/1664) by [Auguste Baum](https://github.com/auguste-probabl) and
  [Riccardo Cappuzzo](https://github.com/rcap107).
- Fixed the use of [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) and [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) with Polars dataframes
  containing a column with empty string as name.
  [#1722](https://github.com/skrub-data/skrub/pull/1722) by [Marie Sacksick](https://github.com/MarieSacksick).
- Fixed an issue where [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) would fail when computing associations
  for Polars dataframes if PyArrow was not installed.
  [#1742](https://github.com/skrub-data/skrub/pull/1742) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Fixed an issue in the Data Ops report generation in cases where the DataOp
  contained escape characters or were spanning multiple lines.
  [#1764](https://github.com/skrub-data/skrub/pull/1764) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Added `get_feature_names_out()` to [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) for consistency with the
  [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and other transformers. [#1762](https://github.com/skrub-data/skrub/pull/1762) by
  [Riccardo Cappuzzo](https://github.com/rcap107).
- Improve error message when [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder) is used without the optional
  transformers dependencies. [#1769](https://github.com/skrub-data/skrub/pull/1769) by [Fangxuan Zhou](https://github.com/fxzhou22).
- Accessing `.skb.applied_estimator` on a [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) after calling
  `.skb.set_name()`, `.skb.set_description()`, `.skb.mark_as_X()` or
  `.skb.mark_as_y()` used to raise an error, this has been fixed in [#1782](https://github.com/skrub-data/skrub/pull/1782)
  by [Jérôme Dockès](https://github.com/jeromedockes).
- Fixed potential issues that could arise in [`ParamSearch.plot_results()`](reference/generated/skrub.ParamSearchhtml.md#skrub.ParamSearch.plot_results)
  when NaN values were present in the cross-validation results.
  [#1800](https://github.com/skrub-data/skrub/pull/1800) by [Riccardo Cappuzzo](https://github.com/rcap107).

## Release 0.6.2

### New features

- The [`DataOp.skb.full_report()`](reference/generated/skrub.DataOp.skb.full_reporthtml.md#skrub.DataOp.skb.full_report) now displays the time each node took to
  evaluate. [#1596](https://github.com/skrub-data/skrub/pull/1596) by [Jérôme Dockès](https://github.com/jeromedockes).

### Changes

- Ken embeddings are now deprecated, the functions `datasets.get_ken_embeddings()`,
  `datasets.get_ken_table_aliases()`, and `datasets.get_ken_types()` will be
  removed in the next release of skrub.
  [#1546](https://github.com/skrub-data/skrub/pull/1546) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- Improved error messages when a DataOp is being sent to dispatched functions.
  [#1607](https://github.com/skrub-data/skrub/pull/1607) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The accepted values for the parameter `how` of [`DataOp.skb.apply()`](reference/generated/skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) have
  changed. The new values are `"auto"` (unchanged), `"cols"` to wrap the
  transformer in [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols), `"frame"` to wrap the transformer in
  `ApplyToFrame`, or `"no_wrap"` for no wrapping. The old values are
  deprecated and will result in an error in a future release.
  [#1628](https://github.com/skrub-data/skrub/pull/1628) by [Jérôme Dockès](https://github.com/jeromedockes).
- The parameter `splitter` of [`DataOp.skb.train_test_split()`](reference/generated/skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split) has been
  renamed `split_func`. [#1630](https://github.com/skrub-data/skrub/pull/1630) by [Jérôme Dockès](https://github.com/jeromedockes).
- KEN embeddings and all the relevant functions have been removed from skrub.
  [#1567](https://github.com/skrub-data/skrub/pull/1567) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The objects `tabular_learner` and `DropIfTooManyNulls` were removed. Use
  [`tabular_pipeline()`](reference/generated/skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline) and [`DropUninformative`](reference/generated/skrub.DropUninformativehtml.md#skrub.DropUninformative) instead.
  [#1567](https://github.com/skrub-data/skrub/pull/1567) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The skrub global configuration now includes a parameter for setting the default
  verbosity of the [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport).
  [#1567](https://github.com/skrub-data/skrub/pull/1567) by [Riccardo Cappuzzo](https://github.com/rcap107).

### Bugfixes

- Fixed a compatibility bug with Polars 1.32.3 that may cause `ToFloat32` to fail
  when applied to categorical columns. [#1570](https://github.com/skrub-data/skrub/pull/1570) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Fixed the display of DataOp objects in google colab cell outputs (no output
  was displayed). [#1590](https://github.com/skrub-data/skrub/pull/1590) by [Jérôme Dockès](https://github.com/jeromedockes).
- Fixed an error that occurred when using `.skb.concat` with a pandas dataframe
  with column names that aren’t strings. [#1594](https://github.com/skrub-data/skrub/pull/1594) by [Riccardo Cappuzzo](https://github.com/rcap107).
- Fixed the range from which [`choose_float()`](reference/generated/skrub.choose_floathtml.md#skrub.choose_float) and [`choose_int()`](reference/generated/skrub.choose_inthtml.md#skrub.choose_int) sample
  values when `log=False` and `n_steps` is `None`. It was between `low`
  and `low + high`, now it is between `low` and `high`. [#1603](https://github.com/skrub-data/skrub/pull/1603) by
  [Jérôme Dockès](https://github.com/jeromedockes).
- DataOp hyperparameter search would raise an error when doing classification
  and using the `scoring` parameter, when the dataop contained no variables.
  Fixed in [#1601](https://github.com/skrub-data/skrub/pull/1601) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) used to do a prediction on the train set during
  `fit()`, this has been fixed.
  [#1610](https://github.com/skrub-data/skrub/pull/1610) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`DataOp`](reference/generated/skrub.DataOphtml.md#skrub.DataOp) would raise errors when containing subclasses of list, tuple
  or dict that cannot be initialized with an instance of the builtin type (such
  as classes created by `collections.namedtuple`), this has been fixed.
  DataOps now only recurse into the builtin collections to evaluate their items
  (not into their subclasses). If you need the items evaluated (ie if they
  contain DataOps or Choices), store them in one of the builtin collections.
  [#1612](https://github.com/skrub-data/skrub/pull/1612) by [Jérôme Dockès](https://github.com/jeromedockes).
- [`SkrubLearner.report()`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.report) with `mode="fit"` used to display the dataops
  themselves, rather than their outputs, in the report. This has been fixed in
  [#1623](https://github.com/skrub-data/skrub/pull/1623) by [Jérôme Dockès](https://github.com/jeromedockes).
- Fixed a bug that happened when `get_feature_names_out` was called on instances
  of the [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder). [#1622](https://github.com/skrub-data/skrub/pull/1622) by [Riccardo Cappuzzo](https://github.com/rcap107).

## Release 0.6.1

### Bugfixes

- `get_feature_names_out` now works correctly when used by [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder),
  [`DropCols`](reference/generated/skrub.DropColshtml.md#skrub.DropCols), `SelectCols:` from within a scikit-learn `Pipeline`. In
  addition, [`DropCols`](reference/generated/skrub.DropColshtml.md#skrub.DropCols)’s `get_feature_names_out` method now returns the
  names of the columns that are not dropped, rather than the names of the columns
  that are dropped. [#1543](https://github.com/skrub-data/skrub/pull/1543) by [Riccardo Cappuzzo](https://github.com/rcap107).

## Release 0.6.0

### Highlights

- Major feature! Skrub DataOps are a powerful new way of
  combining dataframe transformations over multiple tables, and machine learning
  pipelines. DataOps can be combined to form compled data plans, that can be used
  to train and tune machine learning models. Then, the DataOps plans can be exported
  as `Learners` ([`skrub.SkrubLearner`](reference/generated/skrub.SkrubLearnerhtml.md#skrub.SkrubLearner)), standalone objects that can be
  used on new data. More detail about the DataOps can be found in the
  [User guide](data_opshtml.md#user-guide-data-ops-index) and in the
  [examples](auto_examples/02_data_ops/indexhtml.md#data-ops-examples-ref).
- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) has been improved with many new features. Series are
  now supported directly. It is now
  possible to skip computing column associations and generating plots when the
  number of columns in the dataframe exceeds a user-defined threshold. Columns with
  high cardinality and sorted columns are now highlighted in the report.
- [`selectors`](https://docs.python.org/3/library/selectors.html#module-selectors), [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) and `ApplyToFrame` are now available,
  providing utilities for selecting columns to which a transformer should be applied
  in a flexible way. For more details, see the [User guide](modules/multi_column_operations/selectorshtml.md#user-guide-selectors)
  and the example.
- The [`SquashingScaler`](reference/generated/skrub.SquashingScalerhtml.md#skrub.SquashingScaler) has been added: it robustly rescales and smoothly
  clips numeric columns, enabling more robust handling of numeric columns
  with neural networks. See the [example](auto_examples/0100_squashing_scalerhtml.md#sphx-glr-auto-examples-0100-squashing-scaler-py)

### New features

- The skrub DataOps are new mechanism for building machine-learning
  pipelines that handle multiple tables and easily describing their
  hyperparameter spaces. Main PR: [#1233](https://github.com/skrub-data/skrub/pull/1233) by [Jérôme Dockès](https://github.com/jeromedockes).
  Additional work from other contributors can be found
  [here](https://github.com/skrub-data/skrub/issues?q=merged%3A%3C2025-07-24%20label%3Adata_ops):
  [Vincent Maladiere](https://github.com/Vincent-Maladiere) provided very important help by
  trying the DataOps on many use-cases and datasets, providing feedback and
  suggesting improvements, improving the examples (including creating all the
  figures in the examples) and adding jitter to the parallel coordinate plots,
  [Riccardo Cappuzzo](https://github.com/rcap107) experimented with the DataOps,
  suggested improvements and improved the examples, [Gaël Varoquaux](https://github.com/gaelvaroquaux) , [Guillaume Lemaitre](https://github.com/glemaitre), [Adrin Jalali](https://github.com/adrinjalali), [Olivier Grisel](https://github.com/ogrisel) and others participated
  through many discussions in defining the requirements and the public API.
  See [the examples](auto_examples/02_data_ops/indexhtml.md#data-ops-examples-ref) for
  an introduction.
- The [`selectors`](https://docs.python.org/3/library/selectors.html#module-selectors) module provides utilities for selecting columns to which
  a transformer should be applied in a flexible way. The module was created in
  [#895](https://github.com/skrub-data/skrub/pull/895) by [Jérôme Dockès](https://github.com/jeromedockes) and added to the public API
  in [#1341](https://github.com/skrub-data/skrub/pull/1341) by [Jérôme Dockès](https://github.com/jeromedockes).
- The [`DropUninformative`](reference/generated/skrub.DropUninformativehtml.md#skrub.DropUninformative) transformer is now available. This transformer
  employs different heuristics to detect columns that are not likely to bring
  useful information for training a model.
  The current implementation includes detection of columns that contain only a
  single value (constant columns), only missing values, or all unique values (such
  as IDs). [#1313](https://github.com/skrub-data/skrub/pull/1313) by [Riccardo Cappuzzo](https://github.com/rcap107).
- [`get_config()`](reference/generated/skrub.get_confightml.md#skrub.get_config), [`set_config()`](reference/generated/skrub.set_confightml.md#skrub.set_config) and [`config_context()`](reference/generated/skrub.config_contexthtml.md#skrub.config_context) are now available
  to configure settings for dataframes display and expressions. [`patch_display()`](reference/generated/skrub.patch_displayhtml.md#skrub.patch_display)
  and [`unpatch_display()`](reference/generated/skrub.unpatch_displayhtml.md#skrub.unpatch_display) are deprecated and will be removed in the next release
  of skrub. [#1427](https://github.com/skrub-data/skrub/pull/1427) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
  The global configuration includes the parameter `cardinality_threshold` that
  controls the threshold value used to warn user if they have high cardinality
  columns in their dataset. [#1498](https://github.com/skrub-data/skrub/pull/1498) by [rouk1](https://github.com/rouk1).
  Additionally, the parameter `float_precision`
  controls the number of significant digits displayed for floating-point values
  in reports. [#1470](https://github.com/skrub-data/skrub/pull/1470) by [George S](https://github.com/georgescutelnicu).
- Added the [`SquashingScaler`](reference/generated/skrub.SquashingScalerhtml.md#skrub.SquashingScaler), a transformer that
  robustly rescales and smoothly clips numeric columns,
  enabling more robust handling of numeric columns
  with neural networks. [#1310](https://github.com/skrub-data/skrub/pull/1310) by [Vincent Maladiere](https://github.com/Vincent-Maladiere) and
  [David Holzmüller](https://github.com/dholzmueller).
- `datasets.toy_order()` is now available to create a toy dataframe and
  corresponding targets for examples.
  [#1485](https://github.com/skrub-data/skrub/pull/1485) by [Antoine Canaguier-Durand](https://github.com/canag).
- [`ApplyToCols`](reference/generated/skrub.ApplyToColshtml.md#skrub.ApplyToCols) and `ApplyToFrame` are now available to apply transformers
  on a set of columns independently and jointly respectively.
  [#1478](https://github.com/skrub-data/skrub/pull/1478) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).

### Changes

#### WARNING
The default high cardinality encoder for both [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and
`tabular_learner()` (now [`tabular_pipeline()`](reference/generated/skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline)) has been changed from
[`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) to [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder). [#1354](https://github.com/skrub-data/skrub/pull/1354) by
[Riccardo Cappuzzo](https://github.com/rcap107).

- The `tabular_learner` function has been deprecated in favor of [`tabular_pipeline()`](reference/generated/skrub.tabular_pipelinehtml.md#skrub.tabular_pipeline) to honor
  its scikit-learn pipeline cultural heritage, and remove the ambiguity with the data
  ops Learner. [#1493](https://github.com/skrub-data/skrub/pull/1493) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) now exposes the `stop_words` argument, which is passed to the
  underlying vectorizer ([`TfidfVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer),
  or [`HashingVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html#sklearn.feature_extraction.text.HashingVectorizer)). [#1415](https://github.com/skrub-data/skrub/pull/1415) by
  [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- A new parameter `max_association_columns` has been added to the
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) to skip association computation when the number of columns
  exceeds the specified value. [#1304](https://github.com/skrub-data/skrub/pull/1304) by [Victoria Shevchenko](https://github.com/victoris93).
- The `packaging` dependency was removed.
  [#1307](https://github.com/skrub-data/skrub/pull/1307) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
- [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder), [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) and [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) now compute the
  total standard deviation norm during training, which is a global constant, and
  normalize the vector outputs by performing element-wise division on all entries.
  [#1274](https://github.com/skrub-data/skrub/pull/1274) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- The `DropIfTooManyNulls` transformer has been replaced by the
  [`DropUninformative`](reference/generated/skrub.DropUninformativehtml.md#skrub.DropUninformative) transformer and will be removed in a future release.
  [#1313](https://github.com/skrub-data/skrub/pull/1313) by [Riccardo Cappuzzo](https://github.com/rcap107)
- The `concat_horizontal()` function was replaced with `concat()`. Horizontal or vertical concatenation
  is now controlled by the `axis` parameter. [#1334](https://github.com/skrub-data/skrub/pull/1334) by [Parasa V Prajwal](https://github.com/pvprajwal).
- The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) now accept a `datetime_format`
  parameter for specifying the format to use when parsing datetime columns.
  [#1358](https://github.com/skrub-data/skrub/pull/1358) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The `SimpleCleaner` has been removed. use [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) instead. [#1370](https://github.com/skrub-data/skrub/pull/1370) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The periodic encoding for the `day_in_year` has been removed from the [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) as it was
  redundant. The feature itself is still added if the flag is set to `True`. [#1396](https://github.com/skrub-data/skrub/pull/1396) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The naming scheme used for the features generated by [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder), [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder), [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder),
  [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) has been standardized. Now features generated by all encoders have indices in the range
  `[0, n_components-1]`, rather than `[1, n_components]`. Additionally, columns with empty name are assigned a default
  name that depends on the encoder used. [#1405](https://github.com/skrub-data/skrub/pull/1405) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The optional dependencies ‘dev’, ‘doc’, ‘lint’ and ‘test’ have been coalesced into
  ‘dev’. [#1404](https://github.com/skrub-data/skrub/pull/1404) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now supports Series in addition to Dataframes. [#1420](https://github.com/skrub-data/skrub/pull/1420) by [Vitor Pohlenz](https://github.com/vitorpohlenz).
- The [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) now exposes a parameter to convert numeric values to float32. [#1440](https://github.com/skrub-data/skrub/pull/1440) by
  [Riccardo Cappuzzo](https://github.com/rcap107).
- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now shows if columns are sorted. [#1512](https://github.com/skrub-data/skrub/pull/1512) by [Dea María Léon](https://github.com/DeaMariaLeon).

### Bugfixes

- Fixed a bug that caused the [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) and [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder) to raise an exception if the
  input column was a Categorical datatype. [#1401](https://github.com/skrub-data/skrub/pull/1401) by [Riccardo Cappuzzo](https://github.com/rcap107).

### Documentation

A large number of improvements to the examples, docstrings, and the documentation
website have been made. Contributors include [Vincent Maladiere](https://github.com/Vincent-Maladiere),
[Riccardo Cappuzzo](https://github.com/rcap107), [Jérôme Dockès](https://github.com/jeromedockes),
[Gael Varoquaux](https://github.com/gaelvaroquaux), [Gabriela Gómez Jiménez](https://github.com/gabrielapgomezji),
[Sylvain Combettes](https://github.com/sylvaincom), [Frits Hermans](https://github.com/fritshermans),
[Vitor Pohlenz](https://github.com/vitorpohlenz), [Arturo Amor Quiroz](https://github.com/ArturoAmorQ),
[Marie Sacksick](https://github.com/MarieSacksick), [Emilien Battel](https://github.com/emilienbattel09),
[George El Haber](https://github.com/gmhaber), [Antoine Canaguier-Durand](https://github.com/canag), and
[Lionel Kusch](https://github.com/lionelkusch).

## Release 0.5.4

### Maintenance

* Make `skrub` compatible with scikit-learn 1.7.
  [#1434](https://github.com/skrub-data/skrub/pull/1434) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).

## Release 0.5.3

### Changes

- The `SimpleCleaner` has been renamed to [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner). Use of the
  name `SimpleCleaner` is deprecated and will result in an error in some
  future release of skrub. [#1275](https://github.com/skrub-data/skrub/pull/1275) by [Riccardo Cappuzzo](https://github.com/rcap107).
- A new parameter `max_plot_columns` has been added to the
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) and [`patch_display()`](reference/generated/skrub.patch_displayhtml.md#skrub.patch_display) to skip column plots when the
  number of columns exceeds the specified value. [#1255](https://github.com/skrub-data/skrub/pull/1255) by [Priscilla
  Baah](https://github.com/priscilla-b).

## Release 0.5.2

### New features

- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now switches its visual theme between light and dark according to the user preferences.
  [#1201](https://github.com/skrub-data/skrub/pull/1201) by [rouk1](https://github.com/rouk1).
- Adding a new way to control the location of the data directory, using envar `SKRUB_DATA_DIRECTORY`.
  [#1215](https://github.com/skrub-data/skrub/pull/1215) by [Thomas S.](https://github.com/thomass-dev)
- The [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) now supports periodic encoding of datetime features
  with trigonometric functions and B-splines transformers.
  [#1235](https://github.com/skrub-data/skrub/pull/1235) by [Riccardo Cappuzzo](https://github.com/rcap107).
- The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now also compute Pearson’s correlation for numeric values.
  [#1203](https://github.com/skrub-data/skrub/pull/1203) by [Reshama Shaikh](https://github.com/reshamas) and
  [Vincent Maladiere](https://github.com/Vincent-Maladiere).
- The `SimpleCleaner` is now available (⚠️ it was renamed to
  [`Cleaner`](reference/generated/skrub.Cleanerhtml.md#skrub.Cleaner) in skrub `0.5.3`.). This transformer is a lightweight
  pre-processor that applies some of the transformations applied by the
  [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer), with a simpler interface. [#1266](https://github.com/skrub-data/skrub/pull/1266) by
  [Riccardo Cappuzzo](https://github.com/rcap107) and [Jerome Dockes](https://github.com/jeromedockes) .

### Changes

- The estimator returned by `tabular_learner()` now uses spline encoding of
  datetime features when the supervised learner is not a model based on decision
  trees such as random forests or gradient boosting. [#1264](https://github.com/skrub-data/skrub/pull/1264) by
  [Guillaume Lemaitre](https://github.com/glemaitre).
- The “distribution” tab of the `TableReport` now stacks cards horizontally to avoid adding
  vertical space.
  [#1259](https://github.com/skrub-data/skrub/pull/1259) by [Gaël Varoquaux](https://github.com/gaelvaroquaux)
- Progress messages when generating a `TableReport` are now written to stderr instead of stdout.
  [#1236](https://github.com/skrub-data/skrub/pull/1236) by [Priscilla Baah](https://github.com/priscilla-b)
- Optimize the [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder): lower memory footprint and faster execution in some cases.
  [#1248](https://github.com/skrub-data/skrub/pull/1248) by [Gaël Varoquaux](https://github.com/gaelvaroquaux)

### Bug fixes

- [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) now works correctly in presence of null values.
  [#1224](https://github.com/skrub-data/skrub/pull/1224) by [Jérôme Dockès](https://github.com/jeromedockes).
- The [`TableVectorizer.get_feature_names_out()`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer.get_feature_names_out) method now works when used in a
  scikit-learn pipeline by exposing the `input_features` parameter.
  [#1258](https://github.com/skrub-data/skrub/pull/1258) by [Guillaume Lemaitre](https://github.com/glemaitre).

## Release 0.5.1

### New features

* The [`StringEncoder`](reference/generated/skrub.StringEncoderhtml.md#skrub.StringEncoder) encodes strings using tf-idf and truncated SVD
  decomposition and provides a cheaper alternative to [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder).
  [#1159](https://github.com/skrub-data/skrub/pull/1159) by [Riccardo Cappuzzo](https://github.com/rcap107).

### Changes

* New dataset fetching methods have been added: `fetch_videogame_sales()`,
  `fetch_bike_sharing()`, `fetch_flight_delays()`,
  `fetch_country_happiness()`, and removed `fetch_road_safety()`.
  [#1218](https://github.com/skrub-data/skrub/pull/1218) by [Vincent Maladiere](https://github.com/Vincent-Maladiere)

### Bug fixes

### Maintenance

## Release 0.4.1

### Changes

* [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) has `write_html` method. [#1190](https://github.com/skrub-data/skrub/pull/1190) by [Mojdeh Rastgoo](https://github.com/mrastgoo).
* A new parameter `verbose` has been added to the [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) to toggle on or off the
  printing of progress information when a report is being generated.
  [#1182](https://github.com/skrub-data/skrub/pull/1182) by [Priscilla Baah](https://github.com/priscilla-b).
* A parameter `verbose` has been added to the [`patch_display()`](reference/generated/skrub.patch_displayhtml.md#skrub.patch_display) to toggle on or off the
  printing of progress information when a table report is being generated.
  [#1188](https://github.com/skrub-data/skrub/pull/1188) by [Priscilla Baah](https://github.com/priscilla-b).
* `tabular_learner()` accepts the alias `"regression"` for the option
  `"regressor"` and `"classification"` for `"classifier"`.
  [#1180](https://github.com/skrub-data/skrub/pull/1180) by [Mojdeh Rastgoo](https://github.com/mrastgoo).

### Bug fixes

* Generating a `TableReport` could have an effect on the matplotib
  configuration which could cause plots not to display inline in jupyter
  notebooks any more. This has been fixed in skrub in [#1172](https://github.com/skrub-data/skrub/pull/1172) by
  [Jérôme Dockès](https://github.com/jeromedockes) and the matplotlib issue can be tracked
  [here](https://github.com/matplotlib/matplotlib/issues/25041).
* The labels on bar plots in the `TableReport` for columns of object dtypes
  that have a repr spanning multiple lines could be unreadable. This has been
  fixed in [#1196](https://github.com/skrub-data/skrub/pull/1196) by [Jérôme Dockès](https://github.com/jeromedockes).
* Improve the performance of [`deduplicate()`](reference/generated/skrub.deduplicatehtml.md#skrub.deduplicate) by removing some unnecessary
  computations. [#1193](https://github.com/skrub-data/skrub/pull/1193) by [Jérôme Dockès](https://github.com/jeromedockes).

### Maintenance

* Make `skrub` compatible with scikit-learn 1.6.
  [#1169](https://github.com/skrub-data/skrub/pull/1169) by [Guillaume Lemaitre](https://github.com/glemaitre).

## Release 0.4.0

### Highlights

* The [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder) can extract embeddings from a string column with  a deep
  learning language model (possibly downloaded from the HuggingFace Hub).
* Several improvements to the [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) such as better support for
  other scripts than the latin alphabet in the bar plot labels, smaller report
  sizes, clipping the outliers to better see the details of distributions in
  histograms. See the full changelog for details.
* The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) can now drop columns that contain a fraction of
  null values above a user-chosen threshold.

### New features

* The [`TextEncoder`](reference/generated/skrub.TextEncoderhtml.md#skrub.TextEncoder) is now available to encode string columns with
  diverse entries.
  It allows the representation of table entries as embeddings computed by a deep
  learning language model. The weights of this model can be fetched locally
  or from the HuggingFace Hub.
  [#1077](https://github.com/skrub-data/skrub/pull/1077) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
* The [`column_associations()`](reference/generated/skrub.column_associationshtml.md#skrub.column_associations) function has been added. It computes a
  pairwise measure of statistical dependence between all columns in a dataframe
  (the same as shown in the [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport)). [#1109](https://github.com/skrub-data/skrub/pull/1109) by [Jérôme
  Dockès](https://github.com/jeromedockes).
* The [`patch_display()`](reference/generated/skrub.patch_displayhtml.md#skrub.patch_display) function has been added. It changes the display of
  pandas and polars dataframes in jupyter notebooks to replace them with a
  [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport). This can be undone with [`unpatch_display()`](reference/generated/skrub.unpatch_displayhtml.md#skrub.unpatch_display).
  [#1108](https://github.com/skrub-data/skrub/pull/1108) by [Jérôme Dockès](https://github.com/jeromedockes)

### Major changes

* [`AggJoiner`](reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner), [`AggTarget`](reference/generated/skrub.AggTargethtml.md#skrub.AggTarget) and [`MultiAggJoiner`](reference/generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner) now require
  the `operations` argument. They do not split columns by type anymore, but
  apply `operations` on all selected cols. “median” is now supported, “hist” and
  “value_counts” are no longer supported. [#1116](https://github.com/skrub-data/skrub/pull/1116) by [Théo Jolivet](https://github.com/TheooJ).
* The [`AggTarget`](reference/generated/skrub.AggTargethtml.md#skrub.AggTarget) no longer supports `y` inputs of type list. [#1116](https://github.com/skrub-data/skrub/pull/1116)
  by [Théo Jolivet](https://github.com/TheooJ).

### Minor changes

* The column filter selection dropdown in the tablereport is smaller and its
  label has been removed to save space. [#1107](https://github.com/skrub-data/skrub/pull/1107) by [Jérôme Dockès](https://github.com/jeromedockes).
* The TableReport now uses the font size of its parent element when inserted
  into another page. This makes it smaller in pages that use a smaller font size
  than the browser default such as VSCode in some configurations. It also makes
  it easier to control its size when inserting it in a web page by setting the
  font size of its parent element. A few other small adjustments have also been
  made to make it a bit more compact. [#1098](https://github.com/skrub-data/skrub/pull/1098) by [Jérôme Dockès](https://github.com/jeromedockes).
* Display of labels in the plots of the TableReport, especially for other
  scripts than the latin alphabet, has improved.
  - before, some characters could be missing and replaced by empty boxes.
  - before, when the text is truncated, the ellipsis “…” could appear on the
    wrong side for right-to-left scripts.

  Moreover, when the text contains line breaks it now appears all on one line.
  Note this only affects the labels in the plots; the rest of the report did not
  have these problems.
  [#1097](https://github.com/skrub-data/skrub/pull/1097) by [Jérôme Dockès](https://github.com/jeromedockes)
  and [#1138](https://github.com/skrub-data/skrub/pull/1138) by [Jérôme Dockès](https://github.com/jeromedockes).
* In the TableReport it is now possible, before clicking any of the cells, to
  reach the dataframe sample table and activate a cell with tab key navigation.
  [#1101](https://github.com/skrub-data/skrub/pull/1101) by [Jérôme Dockès](https://github.com/jeromedockes).
* The “Column name” column of the “summary statistics” table in the TableReport
  is now always visible when scrolling the table. [#1102](https://github.com/skrub-data/skrub/pull/1102) by [Jérôme
  Dockès](https://github.com/jeromedockes).
* Added parameter `drop_null_fraction` to `TableVectorizer` to drop columns based
  on whether they contain a fraction of nulls larger than the given threshold.
  [#1115](https://github.com/skrub-data/skrub/pull/1115) and [#1149](https://github.com/skrub-data/skrub/pull/1149) by [Riccardo Cappuzzo](https://github.com/rcap107).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now provides more helpful output for columns of dtype
  TimeDelta / Duration. [#1152](https://github.com/skrub-data/skrub/pull/1152) by [Jérôme Dockès](https://github.com/jeromedockes).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) now also reports the number of unique values for
  numeric columns. [#1154](https://github.com/skrub-data/skrub/pull/1154) by [Jérôme Dockès](https://github.com/jeromedockes).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport), when plotting histograms, now detects outliers and
  clips the range of data shown in the histogram. This allows seeing more detail
  in the shown distribution. [#1157](https://github.com/skrub-data/skrub/pull/1157) by [Jérôme Dockès](https://github.com/jeromedockes).

### Bug fixes

* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) could raise an exception when one of the columns
  contained datetimes with time zones and missing values; this has been fixed in
  [#1114](https://github.com/skrub-data/skrub/pull/1114) by [Jérôme Dockès](https://github.com/jeromedockes).
* In scikit-learn versions older than 1.4 the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) could
  fail on polars dataframes when used with the default parameters. This has been
  fixed in [#1122](https://github.com/skrub-data/skrub/pull/1122) by [Jérôme Dockès](https://github.com/jeromedockes).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) would raise an exception when the input (pandas)
  dataframe contained several columns with the same name. This has been fixed in
  [#1125](https://github.com/skrub-data/skrub/pull/1125) by [Jérôme Dockès](https://github.com/jeromedockes).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) would raise an exception when a column contained
  infinite values. This has been fixed in [#1150](https://github.com/skrub-data/skrub/pull/1150) by [Jérôme Dockès](https://github.com/jeromedockes) and [#1151](https://github.com/skrub-data/skrub/pull/1151) by Jérôme Dockès.

## Release 0.3.1

### Minor changes

* For tree-based models, `tabular_learner()` now adds
  `handle_unknown='use_encoded_value'` to the `OrdinalEncoder`, to avoid
  errors with new categories in the test set. This is consistent with the
  setting of `OneHotEncoder` used by default in the
  [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer). [#1078](https://github.com/skrub-data/skrub/pull/1078) by [Gaël Varoquaux](https://github.com/gaelvaroquaux)
* The reports created by [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport), when inserted in an html page (or
  displayed in a notebook), now use the same font as the surrounding page.
  [#1038](https://github.com/skrub-data/skrub/pull/1038) by [Jérôme Dockès](https://github.com/jeromedockes).
* The content of the dataframe corresponding to the currently selected table
  cell in the TableReport can be copied without actually selecting the text (as
  in a spreadsheet).
  [#1048](https://github.com/skrub-data/skrub/pull/1048) by [Jérôme Dockès](https://github.com/jeromedockes).
* The selection of content displayed in the TableReport’s copy-paste boxes has
  been removed. Now they always display the value of the selected item. When
  copied, the repr of the selected item is copied to the clipboard.
  [#1058](https://github.com/skrub-data/skrub/pull/1058) by [Jérôme Dockès](https://github.com/jeromedockes).
* A “stats” panel has been added to the TableReport, showing summary statistics
  for all columns (number of missing values, mean, etc. – similar to
  `pandas.info()` ) in a table. It can be sorted by each column.
  [#1056](https://github.com/skrub-data/skrub/pull/1056) and [#1068](https://github.com/skrub-data/skrub/pull/1068) by [Jérôme Dockès](https://github.com/jeromedockes).
* The credit fraud dataset is now available with the
  `fetch_credit_fraud function()`.
  [#1053](https://github.com/skrub-data/skrub/pull/1053) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
* Added zero padding for column names in [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) to improve column ordering consistency.
  [#1069](https://github.com/skrub-data/skrub/pull/1069) by [Shreekant Nandiyawar](https://github.com/Shree7676).
* The selection in the TableReport’s sample table can now be manipulated with
  the keyboard. [#1065](https://github.com/skrub-data/skrub/pull/1065) by [Jérôme Dockès](https://github.com/jeromedockes).
* The `TableReport` now displays the pandas (multi-)index, and has a better
  display & interaction of pandas columns when the columns are a MultiIndex.
  [#1083](https://github.com/skrub-data/skrub/pull/1083) by [Jérôme Dockès](https://github.com/jeromedockes).
* It is possible to control the number of rows displayed by the TableReport in
  the “sample” tab panel by specifying `n_rows`.
  [#1083](https://github.com/skrub-data/skrub/pull/1083) by [Jérôme Dockès](https://github.com/jeromedockes).
* the `TableReport` used to raise an exception when the dataframe contained
  unhashable types such as python lists. This has been fixed in [#1087](https://github.com/skrub-data/skrub/pull/1087) by
  [Jérôme Dockès](https://github.com/jeromedockes).
* Display’s columns name with the HTML representation of the fitted TableVectorizer.
  This has been fixed in [#1093](https://github.com/skrub-data/skrub/pull/1093) by [Shreekant Nandiyawar](https://github.com/Shree7676).
* AggTarget will now work even when y is a Series and not raise any error.
  This has been fixed in [#1094](https://github.com/skrub-data/skrub/pull/1094) by [Shreekant Nandiyawar](https://github.com/Shree7676).

## Release 0.3.0

### Highlights

* Polars dataframes are now supported across all `skrub` estimators.
* [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) generates an interactive report for a dataframe. This
  [page](https://skrub-data.org/skrub-reports/examples/) regroups some
  precomputed examples.

### Major changes

* The [`InterpolationJoiner`](reference/generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) now supports polars dataframes. [#1016](https://github.com/skrub-data/skrub/pull/1016)
  by [Théo Jolivet](https://github.com/TheooJ).
* The [`TableReport`](reference/generated/skrub.TableReporthtml.md#skrub.TableReport) provides an interactive report on a dataframe’s
  contents: an overview, summary statistics and plots, statistical associations
  between columns. It can be displayed in a jupyter notebook, a browser tab or
  saved as a static HTML page. [#984](https://github.com/skrub-data/skrub/pull/984) by [Jérôme Dockès](https://github.com/jeromedockes).

### Minor changes

* [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) and [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) used to raise an error when columns
  with the same name appeared in the main and auxiliary table (after adding the
  suffix). This is now allowed and a random string is inserted in the duplicate
  column to ensure all names are unique.
  [#1014](https://github.com/skrub-data/skrub/pull/1014) by [Jérôme Dockès](https://github.com/jeromedockes).
* [`AggJoiner`](reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) and [`AggTarget`](reference/generated/skrub.AggTargethtml.md#skrub.AggTarget) could produce outputs whose column
  names varied across calls to `transform` in some cases in the presence of
  duplicate column names, now the output names are always the same.
  [#1013](https://github.com/skrub-data/skrub/pull/1013) by [Jérôme Dockès](https://github.com/jeromedockes).
* In some cases [`AggJoiner`](reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) and [`AggTarget`](reference/generated/skrub.AggTargethtml.md#skrub.AggTarget) inserted a column in
  the output named “index” containing the pandas index of the auxiliary table.
  This has been corrected.
  [#1020](https://github.com/skrub-data/skrub/pull/1020) by [Jérôme Dockès](https://github.com/jeromedockes).

## Release 0.2.0

### Major changes

* The [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) has been adapted to support polars dataframes. [#945](https://github.com/skrub-data/skrub/pull/945) by [Théo Jolivet](https://github.com/TheooJ).
* The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) now consistently applies the same transformation
  across different calls to `transform`. There also have been some breaking
  changes to its functionality: (i) all transformations are now applied
  independently to each column, i.e. it does not perform multivariate
  transformations (ii) in `specific_transformers` the same column may not be
  used twice (go through 2 different transformers).
  [#902](https://github.com/skrub-data/skrub/pull/902) by [Jérôme Dockès](https://github.com/jeromedockes).
* Some parameters of [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) have been renamed:
  `high_cardinality_transformer` → `high_cardinality`,
  `low_cardinality_transformer` → `low_cardinality`,
  `datetime_transformer` → `datetime`, `numeric_transformer` → `numeric`.
  [#947](https://github.com/skrub-data/skrub/pull/947) by [Jérôme Dockès](https://github.com/jeromedockes).
* The [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) and [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) are now a single-column
  transformers: their `fit`, `fit_transform` and `transform` methods
  accept a single column (a pandas or polars Series). Dataframes and numpy
  arrays are not accepted.
  [#920](https://github.com/skrub-data/skrub/pull/920) and [#923](https://github.com/skrub-data/skrub/pull/923) by [Jérôme Dockès](https://github.com/jeromedockes).
* Added the [`MultiAggJoiner`](reference/generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner) that allows to augment a main table with
  multiple auxiliary tables. [#876](https://github.com/skrub-data/skrub/pull/876) by [Théo Jolivet](https://github.com/TheooJ).
* [`AggJoiner`](reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner) now only accepts a single table as an input, and some of its
  parameters were renamed to be consistent with the [`MultiAggJoiner`](reference/generated/skrub.MultiAggJoinerhtml.md#skrub.MultiAggJoiner).
  It now has a `key`` parameter that allows to join main and auxiliary tables that share
  the same column names. [#876](https://github.com/skrub-data/skrub/pull/876) by [Théo Jolivet](https://github.com/TheooJ).
* `tabular_learner()` has been added to easily create a supervised
  learner that works well on tabular data. [#926](https://github.com/skrub-data/skrub/pull/926) by [Jérôme Dockès](https://github.com/jeromedockes).

### Minor changes

* [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) and [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) used to modify their input
  in-place, replacing missing values with a string. They no longer do so. Their
  parameter `handle_missing` has been removed; now missing values are always
  treated as the empty string.
  [#930](https://github.com/skrub-data/skrub/pull/930) by [Jérôme Dockès](https://github.com/jeromedockes).
* The minimum supported python version is now 3.9
  [#939](https://github.com/skrub-data/skrub/pull/939) by [Jérôme Dockès](https://github.com/jeromedockes).
* Skrub supports numpy 2. [#946](https://github.com/skrub-data/skrub/pull/946) by [Jérôme Dockès](https://github.com/jeromedockes).
* `fetch_ken_embeddings()` now add suffix even with the default
  value for the parameter `pca_components`.
  [#956](https://github.com/skrub-data/skrub/pull/956) by [Guillaume Lemaitre](https://github.com/glemaitre).
* [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) now performs some preprocessing (the same as done by the
  [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer), eg trying to parse dates, converting pandas object
  columns with mixed types to a single type) on the joining columns before
  vectorizing them. [#972](https://github.com/skrub-data/skrub/pull/972) by [Jérôme Dockès](https://github.com/jeromedockes).

## skrub release 0.1.1

This is a bugfix release to adapt to the most recent versions of pandas (2.2) and
scikit-learn (1.5). There are no major changes to the functionality of skrub.

## skrub release 0.1.0

### Major changes

* `TargetEncoder` has been removed in favor of
  [`sklearn.preprocessing.TargetEncoder`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.TargetEncoder.html#sklearn.preprocessing.TargetEncoder), available since scikit-learn 1.3.
* [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) and [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) support several ways of rescaling
  distances; `match_score` has been replaced by `max_dist`; bugs which
  prevented the Joiner to consistently vectorize inputs and accept or reject
  matches across calls to transform have been fixed. [#821](https://github.com/skrub-data/skrub/pull/821) by [Jérôme
  Dockès](https://github.com/jeromedockes).
* [`InterpolationJoiner`](reference/generated/skrub.InterpolationJoinerhtml.md#skrub.InterpolationJoiner) was added to join two tables by using
  machine-learning to infer the matching rows from the second table.
  [#742](https://github.com/skrub-data/skrub/pull/742) by [Jérôme Dockès](https://github.com/jeromedockes).
* Pipelines including [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) can now be grid-searched, since
  we can now call `set_params` on the default transformers of [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer).
  [#814](https://github.com/skrub-data/skrub/pull/814) by [Vincent Maladiere](https://github.com/Vincent-Maladiere)
* [`to_datetime()`](reference/generated/skrub.to_datetimehtml.md#skrub.to_datetime) is now available to support pandas.to_datetime
  over dataframes and 2d arrays.
  [#784](https://github.com/skrub-data/skrub/pull/784) by [Vincent Maladiere](https://github.com/Vincent-Maladiere)
* Some parameters of [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) have changed. The goal is to harmonize
  parameters across all estimator that perform join(-like) operations, as
  discussed in [#751](https://github.com/skrub-data/skrub/discussions/751).
  [#757](https://github.com/skrub-data/skrub/pull/757) by [Jérôme Dockès](https://github.com/jeromedockes).
* `dataframe.pd_join()`, `dataframe.pd_aggregate()`,
  `dataframe.pl_join()` and `dataframe.pl_aggregate()`
  are now available in the dataframe submodule.
  [#733](https://github.com/skrub-data/skrub/pull/733) by [Vincent Maladiere](https://github.com/Vincent-Maladiere)
* `FeatureAugmenter` is renamed to [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner).
  [#674](https://github.com/skrub-data/skrub/pull/674) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) and `FeatureAugmenter` can now join on datetime columns.
  [#552](https://github.com/skrub-data/skrub/pull/552) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`Joiner`](reference/generated/skrub.Joinerhtml.md#skrub.Joiner) now supports joining on multiple column keys.
  [#674](https://github.com/skrub-data/skrub/pull/674) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* The signatures of all encoders and functions have been revised to enforce
  cleaner calls. This means that some arguments that could previously be passed
  positionally now have to be passed as keywords.
  [#514](https://github.com/skrub-data/skrub/pull/514) by [Lilian Boulard](https://github.com/LilianBoulard).
* Parallelized the [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) column-wise. Parameters `n_jobs` and `verbose`
  added to the signature. [#582](https://github.com/skrub-data/skrub/pull/582) by [Lilian Boulard](https://github.com/LilianBoulard)
* Introducing [`AggJoiner`](reference/generated/skrub.AggJoinerhtml.md#skrub.AggJoiner), a transformer performing
  aggregation on auxiliary tables followed by left-joining on a base table.
  [#600](https://github.com/skrub-data/skrub/pull/600) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
* Introducing [`AggTarget`](reference/generated/skrub.AggTargethtml.md#skrub.AggTarget), a transformer performing
  aggregation on the target y, followed by left-joining on a base table.
  [#600](https://github.com/skrub-data/skrub/pull/600) by [Vincent Maladiere](https://github.com/Vincent-Maladiere).
* Added the [`SelectCols`](reference/generated/skrub.SelectColshtml.md#skrub.SelectCols) and [`DropCols`](reference/generated/skrub.DropColshtml.md#skrub.DropCols) transformers that allow
  selecting a subset of a dataframe’s columns inside of a pipeline. [#804](https://github.com/skrub-data/skrub/pull/804) by
  [Jérôme Dockès](https://github.com/jeromedockes).

### Minor changes

* [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) doesn’t remove constant features anymore.
  It also supports an ‘errors’ argument to raise or coerce errors during
  transform, and a ‘add_total_seconds’ argument to include the number of
  seconds since Epoch.
  [#784](https://github.com/skrub-data/skrub/pull/784) by [Vincent Maladiere](https://github.com/Vincent-Maladiere)
* Scaling of `matching_score` in [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) is now between 0 and 1; it used to be between 0.5 and 1. Moreover, the division by 0 error that occurred when all rows had a perfect match has been fixed. [#802](https://github.com/skrub-data/skrub/pull/802) by [Jérôme Dockès](https://github.com/jeromedockes).
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) is now able to apply parallelism at the column level rather than the transformer level. This is the default for univariate transformers, like [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder), and [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder).
  [#592](https://github.com/skrub-data/skrub/pull/592) by [Leo Grinsztajn](https://github.com/LeoGrin)
* `inverse_transform` in [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) now works as expected; it used to raise an exception. [#801](https://github.com/skrub-data/skrub/pull/801) by [Jérôme Dockès](https://github.com/jeromedockes).
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) propagate the `n_jobs` parameter to the underlying
  transformers except if the underlying transformer already set explicitly `n_jobs`.
  [#761](https://github.com/skrub-data/skrub/pull/761) by [Leo Grinsztajn](https://github.com/LeoGrin), [Guillaume Lemaitre](https://github.com/glemaitre),
  and [Jerome Dockes](https://github.com/jeromedockes).
* Parallelized the [`deduplicate()`](reference/generated/skrub.deduplicatehtml.md#skrub.deduplicate) function. Parameter `n_jobs`
  added to the signature. [#618](https://github.com/skrub-data/skrub/pull/618) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
  and [Lilian Boulard](https://github.com/LilianBoulard)
* Functions `datasets.fetch_ken_embeddings()`, `datasets.fetch_ken_table_aliases()`
  and `datasets.fetch_ken_types()` have been renamed.
  [#602](https://github.com/skrub-data/skrub/pull/602) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Make `pyarrow` an optional dependencies to facilitate the integration
  with `pyodide`.
  [#639](https://github.com/skrub-data/skrub/pull/639) by [Guillaume Lemaitre](https://github.com/glemaitre).
* Bumped minimal required Python version to 3.10. [#606](https://github.com/skrub-data/skrub/pull/606) by
  [Gael Varoquaux](https://github.com/GaelVaroquaux)
* Bumped minimal required versions for the dependencies:
  - numpy >= 1.23.5
  - scipy >= 1.9.3
  - scikit-learn >= 1.2.1
  - pandas >= 1.5.3 [#613](https://github.com/skrub-data/skrub/pull/613) by [Lilian Boulard](https://github.com/LilianBoulard)
* You can now pass column-specific transformers to [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)
  using the `specific_transformers` argument.
  [#583](https://github.com/skrub-data/skrub/pull/583) by [Lilian Boulard](https://github.com/LilianBoulard).
* Do not support 1-D array (and pandas Series) in [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer). Pass a
  2-D array (or a pandas DataFrame) with a single column instead. This change is for
  compliance with the scikit-learn API.
  [#647](https://github.com/skrub-data/skrub/pull/647) by [Guillaume Lemaitre](https://github.com/glemaitre)
* Fixes a bug in [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) with `remainder`: it is now cloned if it’s
  a transformer so that the same instance is not shared between different
  transformers.
  [#678](https://github.com/skrub-data/skrub/pull/678) by [Guillaume Lemaitre](https://github.com/glemaitre)
* [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) speedup [#680](https://github.com/skrub-data/skrub/pull/680) by [Leo Grinsztajn](https://github.com/LeoGrin)
  - Improved [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder)’s early stopping logic. The parameters `tol` and `min_iter`
    have been removed. The parameter `max_no_improvement` can now be used to control the
    early stopping.
    [#663](https://github.com/skrub-data/skrub/pull/663) by [Simona Maggio](https://github.com/simonamaggio)
    [#593](https://github.com/skrub-data/skrub/pull/593) by  [Lilian Boulard](https://github.com/LilianBoulard)
    [#681](https://github.com/skrub-data/skrub/pull/681) by  [Leo Grinsztajn](https://github.com/LeoGrin)
  - Implementation improvement leading to a ~x5 speedup for each iteration.
  - Better default hyperparameters: `batch_size` now defaults to 1024, and `max_iter_e_steps`
    to 1.
* Removed the `most_frequent` and `k-means` strategies from the [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder).
  These strategy were used for scalability reasons, but we recommend using the [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
  or the [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) instead. [#596](https://github.com/skrub-data/skrub/pull/596) by [Leo Grinsztajn](https://github.com/LeoGrin)
* Removed the `similarity` argument from the [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) constructor,
  as we only support the ngram similarity. [#596](https://github.com/skrub-data/skrub/pull/596) by [Leo Grinsztajn](https://github.com/LeoGrin)
* Added the `analyzer` parameter to the [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) to allow word counts
  for similarity measures. [#619](https://github.com/skrub-data/skrub/pull/619) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* skrub now uses modern type hints introduced in PEP 585.
  [#609](https://github.com/skrub-data/skrub/pull/609) by [Lilian Boulard](https://github.com/LilianBoulard)
* Some bug fixes for [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) ( [#579](https://github.com/skrub-data/skrub/pull/579)):
  - `check_is_fitted` now looks at `"transformers_"` rather than `"columns_"`
  - the default of the `remainder` parameter in the docstring is now `"passthrough"`
    instead of `"drop"` to match the implementation.
  - uint8 and int8 dtypes are now considered as numeric columns.
* Removed the leading “<” and trailing “>” symbols from KEN entities
  and types.
  [#601](https://github.com/skrub-data/skrub/pull/601) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Add `get_feature_names_out` method to [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder).
  [#616](https://github.com/skrub-data/skrub/pull/616) by [Leo Grinsztajn](https://github.com/LeoGrin)
* Removed `requests` from the requirements. [#613](https://github.com/skrub-data/skrub/pull/613) by [Lilian Boulard](https://github.com/LilianBoulard)
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) now handles mixed types columns without failing
  by converting them to string before type inference.
  [#623\`by :user:\`Leo Grinsztajn <LeoGrin>](https://github.com/skrub-data/skrub/pull/623`by :user:`Leo Grinsztajn <LeoGrin>)
* Moved the default storage location of data to the user’s home folder.
  [#652](https://github.com/skrub-data/skrub/pull/652) by [Felix Lefebvre](https://github.com/flefebv) and
  [Gael Varoquaux](https://github.com/GaelVaroquaux)
* Fixed bug when using [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)’s `transform` method on
  categorical columns with missing values.
  [#644](https://github.com/skrub-data/skrub/pull/644) by [Leo Grinsztajn](https://github.com/LeoGrin)
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) never output a sparse matrix by default. This can be changed by
  increasing the `sparse_threshold` parameter. [#646](https://github.com/skrub-data/skrub/pull/646) by [Leo Grinsztajn](https://github.com/LeoGrin)
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) doesn’t fail anymore if an inferred type doesn’t work during transform.
  The new entries not matching the type are replaced by missing values. [#666](https://github.com/skrub-data/skrub/pull/666) by [Leo Grinsztajn](https://github.com/LeoGrin)

- Dataset fetcher [`datasets.fetch_employee_salaries()`](reference/generated/skrub.datasets.fetch_employee_salarieshtml.md#skrub.datasets.fetch_employee_salaries) now has a parameter
  `overload_job_titles` to allow overloading the job titles
  (`employee_position_title`) with the column `underfilled_job_title`,
  which provides some more information about the job title.
  [#581](https://github.com/skrub-data/skrub/pull/581) by [Lilian Boulard](https://github.com/LilianBoulard)

* Fix bugs which was triggered when `extract_until` was “year”, “month”, “microseconds”
  or “nanoseconds”, and add the option to set it to `None` to only extract `total_time`,
  the time from epoch. [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder). [#743](https://github.com/skrub-data/skrub/pull/743) by [Leo Grinsztajn](https://github.com/LeoGrin)

## Before skrub: dirty_cat

Skrub was born from the [dirty_cat](http://dirty-cat.github.io)
package.

## Dirty-cat release 0.4.1

### Major changes

* [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) and `FeatureAugmenter` can now join on numeric columns based on the euclidean distance.
  [#530](https://github.com/skrub-data/skrub/pull/530) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) and `FeatureAugmenter` can perform many-to-many joins on lists of numeric or string key columns.
  [#530](https://github.com/skrub-data/skrub/pull/530) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`GapEncoder.transform()`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder.transform) will not continue fitting of the instance anymore.
  It makes functions that depend on it ([`get_feature_names_out()`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder.get_feature_names_out),
  [`score()`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder.score), etc.) deterministic once fitted.
  [#548](https://github.com/skrub-data/skrub/pull/548) by [Lilian Boulard](https://github.com/LilianBoulard)
* [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) and `FeatureAugmenter` now perform joins on missing values as in `pandas.merge`
  but raises a warning. [#522](https://github.com/skrub-data/skrub/pull/522) and [#529](https://github.com/skrub-data/skrub/pull/529) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Added `get_ken_table_aliases()` and `get_ken_types()` for exploring
  KEN embeddings. [#539](https://github.com/skrub-data/skrub/pull/539) by [Lilian Boulard](https://github.com/LilianBoulard).

### Minor changes

* Improvement of date column detection and date format inference in [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer). The
  format inference now tries to find a format which works for all non-missing values of the column, and only
  tries pandas default inference if it fails.
  [#543](https://github.com/skrub-data/skrub/pull/543) by [Leo Grinsztajn](https://github.com/LeoGrin)
  [#587](https://github.com/skrub-data/skrub/pull/587) by [Leo Grinsztajn](https://github.com/LeoGrin)

## Dirty-cat Release 0.4.0

### Major changes

* `SuperVectorizer` is renamed as [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer), a warning is raised when using the old name.
  [#484](https://github.com/skrub-data/skrub/pull/484) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* New experimental feature: joining tables using [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) by approximate key matching. Matches are based
  on string similarities and the nearest neighbors matches are found for each category.
  [#291](https://github.com/skrub-data/skrub/pull/291) by [Jovan Stojanovic](https://github.com/jovan-stojanovic) and [Leo Grinsztajn](https://github.com/LeoGrin)
* New experimental feature: `FeatureAugmenter`, a transformer
  that augments with [`fuzzy_join()`](reference/generated/skrub.fuzzy_joinhtml.md#skrub.fuzzy_join) the number of features in a main table by using information from auxiliary tables.
  [#409](https://github.com/skrub-data/skrub/pull/409) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Unnecessary API has been made private: everything (files, functions, classes)
  starting with an underscore shouldn’t be imported in your code. [#331](https://github.com/skrub-data/skrub/pull/331) by [Lilian Boulard](https://github.com/LilianBoulard)
* The [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) now supports a `n_jobs` parameter to parallelize
  the hashes computation. [#267](https://github.com/skrub-data/skrub/pull/267) by [Leo Grinsztajn](https://github.com/LeoGrin) and [Lilian Boulard](https://github.com/LilianBoulard).
* New experimental feature: deduplicating misspelled categories using [`deduplicate()`](reference/generated/skrub.deduplicatehtml.md#skrub.deduplicate) by clustering string distances.
  This function works best when there are significantly more duplicates than underlying categories.
  [#339](https://github.com/skrub-data/skrub/pull/339) by [Moritz Boos](https://github.com/mjboos).

### Minor changes

* Add example `Wikipedia embeddings to enrich the data`. [#487](https://github.com/skrub-data/skrub/pull/487) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* **datasets.fetching**: contains a new function `get_ken_embeddings()` that can be used to download Wikipedia
  embeddings and filter them by type.
* **datasets.fetching**: contains a new function `fetch_world_bank_indicator()` that can be used to download indicators
  from the World Bank Open Data platform.
  [#291](https://github.com/skrub-data/skrub/pull/291) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Removed example `Fitting scalable, non-linear models on data with dirty categories`. [#386](https://github.com/skrub-data/skrub/pull/386) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)’s `minhash()` method is no longer public. [#379](https://github.com/skrub-data/skrub/pull/379) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* Fetching functions now have an additional argument `directory`,
  which can be used to specify where to save and load from datasets.
  [#432](https://github.com/skrub-data/skrub/pull/432) by [Lilian Boulard](https://github.com/LilianBoulard)
* Fetching functions now have an additional argument `directory`,
  which can be used to specify where to save and load from datasets.
  [#432](https://github.com/skrub-data/skrub/pull/432) and [#453](https://github.com/skrub-data/skrub/pull/453) by [Lilian Boulard](https://github.com/LilianBoulard)
* The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)’s default `OneHotEncoder` for low cardinality categorical variables now defaults
  to `handle_unknown="ignore"` instead of `handle_unknown="error"` (for sklearn >= 1.0.0).
  This means that categories seen only at test time will be encoded by a vector of zeroes instead of raising an error. [#473](https://github.com/skrub-data/skrub/pull/473) by [Leo Grinsztajn](https://github.com/LeoGrin)

### Bug fixes

* The [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) now considers `None` and empty strings as missing values, rather
  than raising an error. [#378](https://github.com/skrub-data/skrub/pull/378) by [Gael Varoquaux](https://github.com/GaelVaroquaux)

## Dirty-cat Release 0.3.0

### Major changes

* New encoder: [`DatetimeEncoder`](reference/generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder) can transform a datetime column into several numeric columns
  (year, month, day, hour, minute, second, …). It is now the default transformer used
  in the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) for datetime columns. [#239](https://github.com/skrub-data/skrub/pull/239) by [Leo Grinsztajn](https://github.com/LeoGrin)
* The [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) has seen some major improvements and bug fixes:
  - Fixes the automatic casting logic in `transform`.
  - To avoid dimensionality explosion when a feature has two unique values, the default encoder ([`OneHotEncoder`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder)) now drops one of the two vectors (see parameter `drop="if_binary"`).
  - `fit_transform` and `transform` can now return unencoded features, like the [`ColumnTransformer`](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html#sklearn.compose.ColumnTransformer)’s behavior. Previously, a `RuntimeError` was raised.

  [#300](https://github.com/skrub-data/skrub/pull/300) by [Lilian Boulard](https://github.com/LilianBoulard)
* **Backward-incompatible change in the TableVectorizer**:
  To apply `remainder` to features (with the `*_transformer` parameters),
  the value `'remainder'` must be passed, instead of `None` in previous versions.
  `None` now indicates that we want to use the default transformer. [#303](https://github.com/skrub-data/skrub/pull/303) by [Lilian Boulard](https://github.com/LilianBoulard)
* Support for Python 3.6 and 3.7 has been dropped. Python >= 3.8 is now required. [#289](https://github.com/skrub-data/skrub/pull/289) by [Lilian Boulard](https://github.com/LilianBoulard)
* Bumped minimum dependencies:
  - scikit-learn>=0.23
  - scipy>=1.4.0
  - numpy>=1.17.3
  - pandas>=1.2.0 [#299](https://github.com/skrub-data/skrub/pull/299) and [#300](https://github.com/skrub-data/skrub/pull/300) by [Lilian Boulard](https://github.com/LilianBoulard)
* Dropped support for Jaro, Jaro-Winkler and Levenshtein distances.
  - The [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) now exclusively uses `ngram` for similarities,
    and the `similarity` parameter is deprecated. It will be removed in 0.5. [#282](https://github.com/skrub-data/skrub/pull/282) by [Lilian Boulard](https://github.com/LilianBoulard)

### Notes

* The `transformers_` attribute of the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) now contains column
  names instead of column indices for the “remainder” columns. [#266](https://github.com/skrub-data/skrub/pull/266) by [Leo Grinsztajn](https://github.com/LeoGrin)

## Dirty-cat Release 0.2.2

### Bug fixes

* Fixed a bug in the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) causing a [`FutureWarning`](https://docs.python.org/3/library/exceptions.html#FutureWarning)
  when using the `get_feature_names_out()` method. [#262](https://github.com/skrub-data/skrub/pull/262) by [Lilian Boulard](https://github.com/LilianBoulard)

## Dirty-cat Release 0.2.1

### Major changes

* Improvements to the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)
  > - Type detection works better: handles dates, numerics columns encoded as strings, or numeric columns containing strings for missing values.

  [#238](https://github.com/skrub-data/skrub/pull/238) by [Leo Grinsztajn](https://github.com/LeoGrin)
* `get_feature_names()` becomes `get_feature_names_out()`, following changes in the scikit-learn API.
  `get_feature_names()` is deprecated in scikit-learn > 1.0. [#241](https://github.com/skrub-data/skrub/pull/241) by [Gael Varoquaux](https://github.com/GaelVaroquaux)
* Improvements to the [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
  : - It is now possible to fit multiple columns simultaneously with the [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder).
      Very useful when using for instance the [`make_column_transformer()`](https://scikit-learn.org/stable/modules/generated/sklearn.compose.make_column_transformer.html#sklearn.compose.make_column_transformer) function,
      on multiple columns.

  [#243](https://github.com/skrub-data/skrub/pull/243) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)

### Bug-fixes

* Fixed a bug that resulted in the [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) ignoring the analyzer argument. [#242](https://github.com/skrub-data/skrub/pull/242) by [Jovan Stojanovic](https://github.com/jovan-stojanovic)
* [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder)’s `get_feature_names_out` now accepts all iterators, not just lists. [#255](https://github.com/skrub-data/skrub/pull/255) by [Lilian Boulard](https://github.com/LilianBoulard)
* Fixed [`DeprecationWarning`](https://docs.python.org/3/library/exceptions.html#DeprecationWarning) raised by the usage of `distutils.version.LooseVersion`. [#261](https://github.com/skrub-data/skrub/pull/261) by [Lilian Boulard](https://github.com/LilianBoulard)

### Notes

* Remove trailing imports in the [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder).
* Fix typos and update links for website.
* Documentation of the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) and the [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) improved.

## Dirty-cat Release 0.2.0

Also see pre-release 0.2.0a1 below for additional changes.

### Major changes

* Bump minimum dependencies:
  - scikit-learn (>=0.21.0) [#202](https://github.com/skrub-data/skrub/pull/202) by [Lilian Boulard](https://github.com/LilianBoulard)
  - pandas (>=1.1.5) **! NEW REQUIREMENT !** [#155](https://github.com/skrub-data/skrub/pull/155) by [Lilian Boulard](https://github.com/LilianBoulard)
* **datasets.fetching** - backward-incompatible changes to the example
  datasets fetchers:
  - The backend has changed: we now exclusively fetch the datasets from OpenML.
    End users should not see any difference regarding this.
  - The frontend, however, changed a little: the fetching functions stay the same
    but their return values were modified in favor of a more Pythonic interface.
    Refer to the docstrings of functions `dirty_cat.datasets.fetch_*`
    for more information.
  - The example notebooks were updated to reflect these changes. [#155](https://github.com/skrub-data/skrub/pull/155) by [Lilian Boulard](https://github.com/LilianBoulard)
* **Backward incompatible change to** [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder): The [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) now
  only supports two dimensional inputs of shape (N_samples, 1).
  [#185](https://github.com/skrub-data/skrub/pull/185) by [Lilian Boulard](https://github.com/LilianBoulard) and [Alexis Cvetkov](https://github.com/alexis-cvetkov).
* Update `handle_missing` parameters:
  - [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder): the default value “zero_impute” becomes “empty_impute” (see doc).
  - [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder): the default value “” becomes “zero_impute” (see doc).

  [#210](https://github.com/skrub-data/skrub/pull/210) by [Alexis Cvetkov](https://github.com/alexis-cvetkov).
* Add a method “get_feature_names_out” for the [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) and the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer),
  since `get_feature_names` will be depreciated in scikit-learn 1.2. [#216](https://github.com/skrub-data/skrub/pull/216) by [Alexis Cvetkov](https://github.com/alexis-cvetkov)

### Notes

* Removed hard-coded CSV file `dirty_cat/data/FiveThirtyEight_Midwest_Survey.csv`.
* Improvements to the [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer)
  - Missing values are not systematically imputed anymore
  - Type casting and per-column imputation are now learnt during fitting
  - Several bugfixes

  [#201](https://github.com/skrub-data/skrub/pull/201) by [Lilian Boulard](https://github.com/LilianBoulard)

## Dirty-cat Release 0.2.0a1

Version 0.2.0a1 is a pre-release.
To try it, you have to install it manually using:

```default
pip install --pre dirty_cat==0.2.0a1
```

or from the GitHub repository:

```default
pip install git+https://github.com/dirty-cat/dirty_cat.git
```

### Major changes

* Bump minimum dependencies:
  - Python (>= 3.6)
  - NumPy (>= 1.16)
  - SciPy (>= 1.2)
  - scikit-learn (>= 0.20.0)
* [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer): Added automatic transform through the
  [`TableVectorizer`](reference/generated/skrub.TableVectorizerhtml.md#skrub.TableVectorizer) class. It transforms
  columns automatically based on their type. It provides a replacement
  for scikit-learn’s [`ColumnTransformer`](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html#sklearn.compose.ColumnTransformer) simpler to use on heterogeneous
  pandas DataFrame. [#167](https://github.com/skrub-data/skrub/pull/167) by [Lilian Boulard](https://github.com/LilianBoulard)
* **Backward incompatible change to** [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder): The [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) now only
  supports two-dimensional inputs of shape (n_samples, n_features).
  Internally, features are encoded by independent [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) models,
  and are then concatenated into a single matrix.
  [#185](https://github.com/skrub-data/skrub/pull/185) by [Lilian Boulard](https://github.com/LilianBoulard) and [Alexis Cvetkov](https://github.com/alexis-cvetkov).

### Bug-fixes

* Fix `get_feature_names` for scikit-learn > 0.21. [#216](https://github.com/skrub-data/skrub/pull/216) by [Alexis Cvetkov](https://github.com/alexis-cvetkov)

## Dirty-cat Release 0.1.1

### Major changes

### Bug-fixes

* RuntimeWarnings due to overflow in [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder). [#161](https://github.com/skrub-data/skrub/pull/161) by [Alexis Cvetkov](https://github.com/alexis-cvetkov)

## Dirty-cat Release 0.1.0

### Major changes

* [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder): Added online Gamma-Poisson factorization through the
  [`GapEncoder`](reference/generated/skrub.GapEncoderhtml.md#skrub.GapEncoder) class. This method discovers latent categories formed
  via combinations of substrings, and encodes string data as combinations of
  these categories. To be used if interpretability is important. [#153](https://github.com/skrub-data/skrub/pull/153) by [Alexis Cvetkov](https://github.com/alexis-cvetkov)

### Bug-fixes

* Multiprocessing exception in notebook. [#154](https://github.com/skrub-data/skrub/pull/154) by [Lilian Boulard](https://github.com/LilianBoulard)

## Dirty-cat Release 0.0.7

* **MinHashEncoder**: Added `minhash_encoder.py` and `fast_hast.py` files
  that implement minhash encoding through the [`MinHashEncoder`](reference/generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) class.
  This method allows for fast and scalable encoding of string categorical
  variables.
* **datasets.fetch_employee_salaries**: change the origin of download for employee_salaries.
  - The function now return a bunch with a dataframe under the field “data”,
    and not the path to the csv file.
  - The field “description” has been renamed to “DESCR”.
* **SimilarityEncoder**: Fixed a bug when using the Jaro-Winkler distance as a
  similarity metric. Our implementation now accurately reproduces the behaviour
  of the `python-Levenshtein` implementation.
* **SimilarityEncoder**: Added a `handle_missing` attribute to allow encoding
  with missing values.
* **TargetEncoder**: Added a `handle_missing` attribute to allow encoding
  with missing values.
* **MinHashEncoder**: Added a `handle_missing` attribute to allow encoding
  with missing values.

## Dirty-cat Release 0.0.6

* **SimilarityEncoder**: Accelerate `SimilarityEncoder.transform`, by:
  - computing the vocabulary count vectors in `fit` instead of `transform`
  - computing the similarities in parallel using `joblib`. This option can be
    turned on/off via the `n_jobs` attribute of the [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder).
* **SimilarityEncoder**: Fix a bug that was preventing a [`SimilarityEncoder`](reference/generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder)
  to be created when `categories` was a list.
* **SimilarityEncoder**: Set the dtype passed to the ngram similarity
  to float32, which reduces memory consumption during encoding.

## Dirty-cat Release 0.0.5

* **SimilarityEncoder**: Change the default ngram range to (2, 4) which
  performs better empirically.
* **SimilarityEncoder**: Added a `most_frequent` strategy to define
  prototype categories for large-scale learning.
* **SimilarityEncoder**: Added a `k-means` strategy to define prototype
  categories for large-scale learning.
* **SimilarityEncoder**: Added the possibility to use hashing ngrams for
  stateless fitting with the ngram similarity.
* **SimilarityEncoder**: Performance improvements in the ngram similarity.
* **SimilarityEncoder**: Expose a `get_feature_names` method.
