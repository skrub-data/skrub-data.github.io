# patch_display

### skrub.patch_display(pandas=True, polars=True, verbose=1, plot_distributions=False, compute_associations=False)

Replace the default DataFrame HTML displays with `skrub.TableReport`.

This function replaces the HTML displays (what is shown when an object is
the output of a jupyter notebook cell) of pandas and polars DataFrames
with a TableReport.

It can be undone with `skrub.unpatch_display()`.

* **Parameters:**
  **pandas**
  : If False, do not override the displays for pandas dataframes.

  **polars**
  : If False, do not override the displays for polars dataframes.

  **verbose**
  : Whether to print progress information while table report is being generated.
    * verbose = 1 prints how many columns have been processed so far.
    * verbose = 0 silences the output.

  **plot_distributions**
  : Whether to plot distributions in [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport).
    - `True`: always generate plots, regardless of column count.
    - `False`: never generate plots.
    - `"auto"`: generate plots only when the number of columns
    > does not exceed the configured `plots_threshold` (see [`set_config()`](skrub.set_confightml.md#skrub.set_config)).

  **compute_associations**
  : Whether to compute associations in [`TableReport`](skrub.TableReporthtml.md#skrub.TableReport).
    - `True`: always compute associations, regardless of column count.
    - `False`: never compute associations.
    - `"auto"`: compute associations only when the number of
    > columns does not exceed the configured `associations_threshold`
    > (see [`set_config()`](skrub.set_confightml.md#skrub.set_config)).

#### SEE ALSO
[`unpatch_display`](skrub.unpatch_displayhtml.md#skrub.unpatch_display)
: Undo the change made by this function.

[`TableReport`](skrub.TableReporthtml.md#skrub.TableReport)
: Directly create a report from a dataframe.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div>
<!-- thumbnail-parent-div-close --></div>
