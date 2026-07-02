# set_config

### skrub.set_config(use_table_report_data_ops=None, table_report_plots_threshold=None, table_report_associations_threshold=None, table_report_verbosity=None, subsampling_seed=None, max_plot_columns=None, max_association_columns=None, enable_subsampling=None, float_precision=None, cardinality_threshold=None, data_dir=None, eager_data_ops=None, data_ops_open_graph_dropdown=None)

Set global skrub configuration.

* **Parameters:**
  **use_table_report_data_ops**
  : The type of HTML representation used for the dataframes preview in skrub
    DataOps. If `None`, falls back to the current configuration, which is `True`
    by default.
    - If `True`, [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport) will be used.
    - If `False`, the original Pandas or Polars dataframe display will be
      used.
    <br/>
    This configuration can also be set with the `SKB_USE_TABLE_REPORT_DATA_OPS`
    environment variable.

  **table_report_plots_threshold**
  : Maximum number of columns for which distribution plots are generated
    in [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport) when `plot_distributions="auto"`
    (the default). Dataframes with more columns will skip plots.
    Default is 30.
    <br/>
    This configuration can also be set with the `SKB_TABLE_REPORT_PLOTS_THRESHOLD`
    environment variable.

  **table_report_associations_threshold**
  : Maximum number of columns for which associations are computed
    in [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport) when `compute_associations="auto"`
    (the default). Dataframes with more columns will skip associations.
    Default is 30.
    <br/>
    This configuration can also be set with the
    `SKB_TABLE_REPORT_ASSOCIATIONS_THRESHOLD` environment variable.

  **table_report_verbosity**
  : Set the level of verbosity of the [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport).
    Default is 1 (print the progress bar). Refer to the `TableReport`
    documentation for more details.

  **subsampling_seed**
  : Set the random seed of subsampling in skrub DataOps
    [`skrub.DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample), when `how="random"` is passed.
    <br/>
    This configuration can also be set with the `SKB_SUBSAMPLING_SEED` environment
    variable.

  **enable_subsampling**
  : Control the activation of subsampling in skrub DataOps
    [`skrub.DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample). Default is `"default"`.
    - If `"default"`, the behavior of [`skrub.DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample) is used.
    - If `"disable"`, subsampling is never used, so `skb.subsample` becomes a
      no-op.
    - If `"force"`, subsampling is used in all DataOps evaluation modes
      ([`eval()`](skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval), fit_transform, etc.).
    <br/>
    This configuration can also be set with the `SKB_ENABLE_SUBSAMPLING`
    environment variable.

  **float_precision**
  : Control the number of significant digits shown when formatting floats.
    Applies overall precision rather than fixed decimal places. Default is 3.
    <br/>
    This configuration can also be set with the `SKB_FLOAT_PRECISION`
    environment variable.

  **cardinality_threshold**
  : Set the `cardinality_threshold` argument of [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer).
    Control the threshold value used to warn user if they have
    high cardinality columns in there dataset.
    <br/>
    This configuration can also be set with the `SKB_CARDINALITY_THRESHOLD`
    environment variable.

  **data_dir**
  : Set the data directory path for skrub datasets. If `None`, falls back to
    the current configuration.
    - If the `SKB_DATA_DIRECTORY` environment variable is set to an absolute
      path, that path will be used.
    - Otherwise, the default is `~/skrub_data`.
    <br/>
    This configuration can also be set with the `SKB_DATA_DIRECTORY`
    environment variable. The deprecated `SKRUB_DATA_DIRECTORY` is still
    supported with a deprecation warning.

  **eager_data_ops**
  : Eagerly perform checks on the DataOps as soon they are created, and
    compute previews if preview data is available. If disabled, those
    checks are delayed until the DataOp is actually used (e.g. by calling
    `.skb.eval()` or `make_learner()`), and previews are not computed.
    <br/>
    This option is used to speed-up the creation of large DataOps
    containing many nodes. It can also be useful in rare cases where a
    DataOp needs no inputs (for example it relies on a hard-coded filename
    to load data) but we want to prevent it from computing preview results
    as soon as it is constructed and delay computation until we explicitly
    request it. For most DataOps that do need inputs (contain
    `skrub.var()` nodes), previews can also be disabled simply by not
    providing preview data to `skrub.var()`.
    <br/>
    This configuration can also be set with the `SKB_EAGER_DATA_OPS`
    environment variable.

  **data_ops_open_graph_dropdown**
  : When displaying a DataOp that has a preview value in a jupyter
    notebook, should the dropdown that reveals the computational graph
    drawing be open (if True) or close (if False). This option mostly
    exists to control the display of DataOps in the skrub documentation
    examples. This configuration can also be set with the
    `SKB_DATA_OPS_OPEN_GRAPH_DROPDOWN` environment variable.

#### SEE ALSO
[`get_config`](skrub.get_confightml.md#skrub.get_config)
: Retrieve current values for global configuration.

[`config_context`](skrub.config_contexthtml.md#skrub.config_context)
: Context manager for global skrub configuration.

### Examples

```pycon
>>> from skrub import set_config
>>> set_config(use_table_report_data_ops=True)
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
