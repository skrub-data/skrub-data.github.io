<a id="user-guide"></a>

# User Guide

Skrub is a Python library that facilitates machine learning with tabular data
(dataframes, such as pandas and polars) using a scikit-learn-compatible API.

Use the sections below to navigate the guide. For a quickstart example,
try [Getting Started](auto_tutorials/0000_getting_startedhtml.md#sphx-glr-auto-tutorials-0000-getting-started-py).
For runnable code, see the [Example gallery](auto_examples/indexhtml.md).
For class and function details, see the [API Reference](reference/indexhtml.md#api-ref).
For common use cases and how to address them, see the [How-to guides](howtohtml.md#how-to).

<!-- File to ..include in a document with a big table of content, to give
it 'style' --> <script>
 window.addEventListener('DOMContentLoaded', function() {
      (function($) {
 //Function to make the index toctree collapsible
 $(function () {
     $('main .toctree-l2')
         .click(function(event){
             if (event.target.tagName.toLowerCase() != "a") {
                 if ($(this).children('ul').length > 0) {
                      $(this).attr('data-content',
                          (!$(this).children('ul').is(':hidden')) ? '\\u25ba' : '\\u25bc');
                     $(this).children('ul').toggle();
                 }
                 return true; //Makes links clickable
             }
         })
         .mousedown(function(event){ return false; }) //Firefox highlighting fix
         .children('ul').hide();
     // Initialize the values
     $('main li.toctree-l2:not(:has(ul))').attr('data-content', '-');
     $('main li.toctree-l2:has(ul)').attr('data-content', '\\u25ba');
     $('main li.toctree-l2:has(ul)').css('cursor', 'pointer');

     $('main .toctree-l2').hover(
         function () {
             if ($(this).children('ul').length > 0) {
                 $(this).css('background-color', '#88888844');
                 $(this).attr('data-content',
                     (!$(this).children('ul').is(':hidden')) ? '\\u25bc' : '\\u25ba');
             }
             else {
                 $(this).css('background-color', '#88888844');
             }
         },
         function () {
             $(this).css('background-color', '#88888800');
             if ($(this).children('ul').length > 0) {
                 $(this).attr('data-content',
                     (!$(this).children('ul').is(':hidden')) ? '\\u25bc' : '\\u25ba');
             }
         }
     );
 });
      })(jQuery);
  });
 </script>

<style type="text/css">
  main li, div.body ul {
      transition-duration: 0.2s;
  }

  main li.toctree-l1 {
      padding: 5px 0 0;
      list-style-type: none;
      font-size: 150%;
      font-weight: normal;
      margin-left: 0;
      margin-bottom: 1.2em;
      font-weight: bold;
      }

  main li.toctree-l1 > a {
      margin-left: 0px;
  }

  main li.toctree-l1 ul {
      padding-inline-start: 0em;
  }

  main li.toctree-l2 {
      padding: 0.25em 0 0.25em 0 ;
      list-style-type: none;
      font-size: 85% ;
      font-weight: normal;
      margin-left: 0;
  }

  main li.toctree-l2 ul {
      padding-left: 30px ;
  }

  main li.toctree-l2:before {
      content: attr(data-content);
      font-size: 1rem;
      color: #777;
      display: inline-block;
      width: 1.5rem;
      margin-left: -1.5rem;
  }

  main li.toctree-l3 {
      font-size: 88% ;
      font-weight: normal;
      margin-left: 0;
  }

  main li.toctree-l4 {
      font-size: 93% ;
      font-weight: normal;
      margin-left: 0;
  }

  main div.topic li.toctree-l1 {
      font-size: 100% ;
      font-weight: bold;
      background-color: transparent;
      margin-bottom: 0;
      margin-left: 1.5em;
      display:inline;
  }

  main div.topic p {
      font-size: 90% ;
      margin: 0.4ex;
  }

  main div.topic p.topic-title {
      display:inline;
      font-size: 100% ;
      margin-bottom: 0;
  }

  min li {
      list-style-type: none;
  }

  main div.toctree-wrapper ul {
      padding-left: 0;
  }

  main li.toctree-l1 {
      padding: 0 0 0.5em 0;
      font-size: 150%;
      font-weight: bold;
  }

  main li.toctree-l2 {
      font-size: 70%;
      font-weight: normal;
      margin-left: 30px;
  }

  main li.toctree-l3 {
      font-size: 85%;
      font-weight: normal;
      margin-left: 30px;
  }

  main li.toctree-l4 {
      margin-left: 30px;
  }

</style>

* [Getting Started with skrub](auto_tutorials/0000_getting_startedhtml.md)
  * [Preliminary exploration with the `TableReport`](auto_tutorials/0000_getting_startedhtml.md#preliminary-exploration-with-the-tablereport)
  * [Sanitizing data with the `Cleaner`](auto_tutorials/0000_getting_startedhtml.md#sanitizing-data-with-the-cleaner)
  * [Easily building a strong baseline for tabular machine learning](auto_tutorials/0000_getting_startedhtml.md#easily-building-a-strong-baseline-for-tabular-machine-learning)
  * [Encoding any data as numerical features](auto_tutorials/0000_getting_startedhtml.md#encoding-any-data-as-numerical-features)
  * [Advanced use cases](auto_tutorials/0000_getting_startedhtml.md#advanced-use-cases)
  * [Next steps](auto_tutorials/0000_getting_startedhtml.md#next-steps)
* [Exploring a Dataframe](exploring_a_dataframehtml.md)
  * [Exploring dataframes interactively with the `TableReport`](modules/tablereport/exploring_dataframes_interactivelyhtml.md)
    * [A demo of the `TableReport`](modules/tablereport/exploring_dataframes_interactivelyhtml.md#a-demo-of-the-tablereport)
* [Wrangling data with good defaults](default_wranglinghtml.md)
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
* [Column-level feature extraction](column_level_featurizinghtml.md)
  * [Encoding string and text columns as numeric features](modules/column_level_featurizing/feature_engineering_categoricalhtml.md)
    * [Choosing the right encoder for the job](modules/column_level_featurizing/feature_engineering_categoricalhtml.md#choosing-the-right-encoder-for-the-job)
  * [Handling datetimes: parsing from strings and encoding as numbers](modules/column_level_featurizing/feature_engineering_datetimeshtml.md)
    * [Parsing Datetime Strings with `ToDatetime`](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#parsing-datetime-strings-with-todatetime)
    * [Encoding and Feature Engineering with `DatetimeEncoder`](modules/column_level_featurizing/feature_engineering_datetimeshtml.md#encoding-and-feature-engineering-with-datetimeencoder)
  * [Parsing and scaling numeric features](modules/column_level_featurizing/feature_engineering_numericalhtml.md)
    * [Converting heterogeneous numeric values to uniform float32](modules/column_level_featurizing/feature_engineering_numericalhtml.md#converting-heterogeneous-numeric-values-to-uniform-float32)
    * [Robust scaling of numeric features using `SquashingScaler`](modules/column_level_featurizing/feature_engineering_numericalhtml.md#robust-scaling-of-numeric-features-using-squashingscaler)
  * [Advanced columnwise operations](modules/column_level_featurizing/advanced_columnwise_operationshtml.md)
    * [The single column transformer](modules/column_level_featurizing/advanced_columnwise_operationshtml.md#the-single-column-transformer)
    * [Rejection handling with `ApplyToCols` and `core.RejectColumn`](modules/column_level_featurizing/advanced_columnwise_operationshtml.md#rejection-handling-with-applytocols-and-rejectcolumn)
* [Multi-column operations](multi_column_operationshtml.md)
  * [Removing unneeded columns with `DropUninformative` and `Cleaner`](modules/multi_column_operations/drop_uninformativehtml.md)
  * [Dropping columns with many missing values](modules/multi_column_operations/drop_uninformativehtml.md#dropping-columns-with-many-missing-values)
  * [Applying `DropUninformative` only to a subset of columns](modules/multi_column_operations/drop_uninformativehtml.md#applying-dropuninformative-only-to-a-subset-of-columns)
  * [Skrub Selectors, for selecting columns in a dataframe](modules/multi_column_operations/selectorshtml.md)
    * [Introduction to selectors](modules/multi_column_operations/selectorshtml.md#introduction-to-selectors)
    * [Type of selectors](modules/multi_column_operations/selectorshtml.md#type-of-selectors)
    * [Combining selectors](modules/multi_column_operations/selectorshtml.md#combining-selectors)
    * [Visualizing a selector](modules/multi_column_operations/selectorshtml.md#visualizing-a-selector)
    * [Using selectors with other skrub transformers](modules/multi_column_operations/selectorshtml.md#using-selectors-with-other-skrub-transformers)
  * [Selecting based on dtype or data properties](modules/multi_column_operations/type_of_selectorshtml.md)
  * [Categories of selectors](modules/multi_column_operations/type_of_selectorshtml.md#categories-of-selectors)
    * [Selectors based on column data types](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-data-types)
    * [Selectors based on column content and properties](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-content-and-properties)
    * [Selectors based on column names](modules/multi_column_operations/type_of_selectorshtml.md#selectors-based-on-column-names)
  * [`filter()` and `filter_names()` to select with user-defined criteria](modules/multi_column_operations/advanced_selectorshtml.md)
    * [Example of custom criteria in `filter()`: selecting columns with outliers](modules/multi_column_operations/advanced_selectorshtml.md#example-of-custom-criteria-in-filter-selecting-columns-with-outliers)
  * [Select columns with null values](modules/multi_column_operations/advanced_selectorshtml.md#select-columns-with-null-values)
    * [Example: Selecting columns by null percentage with `has_nulls()`](modules/multi_column_operations/advanced_selectorshtml.md#example-selecting-columns-by-null-percentage-with-has-nulls)
  * [Detecting sessions in timestamped data with the SessionEncoder](modules/multi_column_operations/sessionizationhtml.md)
* [Building complete pipelines with DataOps](data_opshtml.md)
  * [A quick overview of DataOps](data_opshtml.md#a-quick-overview-of-dataops)
    * [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tourhtml.md)
  * [Data Ops basic concepts](data_opshtml.md#data-ops-basic-concepts)
    * [Basics of DataOps: the DataOps plan, variables, and learners](modules/data_ops/basics/what_are_data_opshtml.md)
    * [Building a simple DataOps plan](modules/data_ops/basics/building_data_ops_planhtml.md)
    * [Using previews for easier development and debugging](modules/data_ops/basics/using_previewshtml.md)
    * [DataOps allow direct access to methods of the underlying data](modules/data_ops/basics/direct_access_methodshtml.md)
    * [Control flow in DataOps: eager and deferred evaluation](modules/data_ops/basics/control_flowhtml.md)
    * [How do skrub Data Ops differ from the alternatives?](modules/data_ops/basics/data_ops_vs_alternativeshtml.md)
  * [Building a complex pipeline with the skrub Data Ops](data_opshtml.md#building-a-complex-pipeline-with-the-skrub-data-ops)
    * [Applying machine-learning estimators](modules/data_ops/ml_pipeline/applying_ml_estimatorshtml.md)
    * [Applying different transformers using skrub selectors and DataOps](modules/data_ops/ml_pipeline/applying_different_transformershtml.md)
    * [Documenting the DataOps plan with node names and descriptions](modules/data_ops/ml_pipeline/documenting_data_ops_planhtml.md)
    * [Evaluating and debugging the DataOps plan with `.skb.full_report()`](modules/data_ops/ml_pipeline/evaluating_debugging_data_opshtml.md)
    * [Using only a part of a DataOps plan](modules/data_ops/ml_pipeline/using_part_of_data_ops_planhtml.md)
    * [Subsampling data for easier development and debugging](modules/data_ops/ml_pipeline/subsampling_datahtml.md)
  * [Tuning and validating skrub DataOps plans](data_opshtml.md#tuning-and-validating-skrub-dataops-plans)
    * [Tuning and validating skrub DataOps plans](modules/data_ops/validation/tuning_validating_data_opshtml.md)
    * [Improving the confidence in our score through cross-validation](modules/data_ops/validation/tuning_validating_data_opshtml.md#improving-the-confidence-in-our-score-through-cross-validation)
    * [Splitting the data in train and test sets](modules/data_ops/validation/tuning_validating_data_opshtml.md#splitting-the-data-in-train-and-test-sets)
    * [Passing additional arguments to the splitter](modules/data_ops/validation/tuning_validating_data_opshtml.md#passing-additional-arguments-to-the-splitter)
    * [Passing additional arguments to the scorer](modules/data_ops/validation/tuning_validating_data_opshtml.md#passing-additional-arguments-to-the-scorer)
    * [Avoiding computing predictions multiple times](modules/data_ops/validation/tuning_validating_data_opshtml.md#avoiding-computing-predictions-multiple-times)
    * [Using the skrub `choose_*` functions to tune hyperparameters](modules/data_ops/validation/hyperparameter_tuninghtml.md)
    * [Feature selection with skrub `SelectCols` and `DropCols`](modules/data_ops/validation/hyperparameter_tuninghtml.md#feature-selection-with-skrub-selectcols-and-dropcols)
    * [Validating hyperparameter search with nested cross-validation](modules/data_ops/validation/nested_cross_validationhtml.md)
    * [Going beyond estimator hyperparameters: nesting choices and choosing pipelines](modules/data_ops/validation/nesting_choices_choosing_pipelineshtml.md)
    * [Linking choices depending on other choices](modules/data_ops/validation/nesting_choices_choosing_pipelineshtml.md#linking-choices-depending-on-other-choices)
    * [Exporting the DataOps plan as a learner and reusing it](modules/data_ops/validation/exporting_data_opshtml.md)
    * [Tuning DataOps with Optuna](modules/data_ops/validation/tuning_with_optunahtml.md)
* [Joining Dataframes](joining_dataframeshtml.md)
  * [Assembling: joining multiple tables](modules/joining_tables/assemblinghtml.md)
    * [Joining external tables for machine learning](modules/joining_tables/assemblinghtml.md#joining-external-tables-for-machine-learning)
    * [Fuzzy joining tables](modules/joining_tables/assemblinghtml.md#fuzzy-joining-tables)
