<a id="user-guide-building-pipeline-index"></a>

# Wrangling data with good defaults

This section covers how to build a predictive pipeline starting from a dataframe.
The skrub objects described in this section can be used as strong defaults for
building baseline pipelines, and can be customized for specific use cases.

* [`Cleaner`: sanitizing a dataframe](modules/default_wrangling/cleaning_dataframeshtml.md)
  * [Parsing numeric-looking strings with the `Cleaner`](modules/default_wrangling/cleaning_dataframeshtml.md#parsing-numeric-looking-strings-with-the-cleaner)
  * [Downcasting float dtypes to `float32` with the `Cleaner`](modules/default_wrangling/cleaning_dataframeshtml.md#downcasting-float-dtypes-to-float32-with-the-cleaner)
* [Transforming a table into an array of numeric features: `TableVectorizer`](modules/default_wrangling/table_vectorizerhtml.md)
  * [Numeric strings and categorical encoding](modules/default_wrangling/table_vectorizerhtml.md#numeric-strings-and-categorical-encoding)
* [Building robust ML baselines with `tabular_pipeline()`](modules/default_wrangling/tabular_pipelinehtml.md)
* [The logic used by the tabular pipeline is quite simple](modules/default_wrangling/tabular_pipelinehtml.md#the-logic-used-by-the-tabular-pipeline-is-quite-simple)
* [Extending the pipeline with the `.steps` attribute](modules/default_wrangling/tabular_pipelinehtml.md#extending-the-pipeline-with-the-steps-attribute)
* [Using a pipeline as the estimator](modules/default_wrangling/tabular_pipelinehtml.md#using-a-pipeline-as-the-estimator)
* [Transforming selected columns with `ApplyToCols`](modules/default_wrangling/apply_to_colshtml.md)
  * [Dealing with columns that cannot be handled by a transformer](modules/default_wrangling/apply_to_colshtml.md#dealing-with-columns-that-cannot-be-handled-by-a-transformer)
  * [Advanced usage of `ApplyToCols`](modules/default_wrangling/apply_to_colshtml.md#advanced-usage-of-applytocols)
