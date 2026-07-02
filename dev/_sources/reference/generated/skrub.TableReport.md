# TableReport

### *class* skrub.TableReport(dataframe, n_rows=10, order_by=None, title=None, column_filters=None, verbose=None, plot_distributions='auto', compute_associations='auto', open_tab='table', max_plot_columns=None, max_association_columns=None)

Summarize the contents of a dataframe.

This class summarizes a dataframe or numpy array, providing information such as
the type and summary statistics (mean, number of missing values, etc.) for each
column. Numpy arrays are converted to pandas DataFrame or Series. The computed
statistics can be accessed interactively in a Jupyter notebook or web browser.
Alternatively, it can be saved or exported in JSON, Markdown, or HTML format
for programmatic access or for inclusion in documents.

* **Parameters:**
  **dataframe**
  : The dataframe or series to summarize.

  **n_rows**
  : Maximum number of rows to show in the sample table. Half will be taken
    from the beginning (head) of the dataframe and half from the end
    (tail). Note this is only for display. Summary statistics, histograms
    etc. are computed using the whole dataframe.

  **order_by**
  : Deprecated. Column name to use for sorting. Other numerical columns
    will be plotted as function of the sorting column. Must be of
    numerical or datetime type.
    <br/>
    #### Deprecated
    Deprecated since version 0.10.0.

  **title**
  : Title for the report.

  **column_filters**
  : A dict for adding custom entries to the column filter dropdown menu.
    Each key is the filter named to be displayed in the dropdown menu
    (e.g. `"first_10"`), and the value is the desired filter. Allowed
    formats for the filter values are a list of column names,
    a list of column indices, or a Selector object.
    See the end of the “Examples” section below for details.

  **verbose**
  : Whether to print progress information while the report is being generated.
    * verbose = `None` uses the global configuration (see [`set_config()`](skrub.set_confightml.md#skrub.set_config)),
      which then defaults to 1.
    * verbose = 1 prints how many columns have been processed so far.
    * verbose = 0 silences the output.

  **plot_distributions**
  : Whether to plot the distributions of the columns.
    - `True`: always generate plots, regardless of column count.
    - `False`: never generate plots.
    - `"auto"` (default): generate plots only when the number of columns
      does not exceed the configured `table_report_plots_threshold`
      (see [`set_config()`](skrub.set_confightml.md#skrub.set_config)).

  **compute_associations**
  : Whether to compute associations between columns.
    - `True`: always compute associations, regardless of column count.
    - `False`: never compute associations.
    - `"auto"` (default): compute associations only when the number of
      columns does not exceed the configured `table_report_associations_threshold`
      (see [`set_config()`](skrub.set_confightml.md#skrub.set_config)).

  **max_plot_columns**
  : Deprecated in favor of `plot_distributions`. This parameter overrides
    the value chosen for `plot_distributions` when it is not None.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0.

  **max_association_columns**
  : Deprecated in favor of `compute_associations`. This parameter overrides
    the value chosen for `compute_associations` when it is not None.
    <br/>
    #### Deprecated
    Deprecated since version 0.9.0.

  **open_tab**
  : The tab that will be displayed by default when the report is opened.
    Must be one of “table”, “stats”, “distributions”, or “associations”.
    * “table”: Shows a sample of the dataframe rows
    * “stats”: Shows summary statistics for all columns
    * “distributions”: Shows plots of column distributions
    * “associations”: Shows column associations and similarities

#### SEE ALSO
[`patch_display`](skrub.patch_displayhtml.md#skrub.patch_display)
: Replace the default DataFrame HTML displays in the output of notebook cells with a TableReport.

### Notes

You can see some [example reports](https://skrub-data.org/skrub-reports/examples/) for a few datasets online. We also
provide an experimental online [demo](https://skrub-data.org/skrub-reports/) that allows you to select a CSV or
parquet file and generate a report directly in your web browser.

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import TableReport
>>> df = pd.DataFrame(dict(a=[1, 2], b=['one', 'two'], c=[11.1, 11.1]))
>>> report = TableReport(df)
```

If you are in a Jupyter notebook, to display the report just have it be the
last expression evaluated in a cell so that it is displayed in the cell’s
output.

```pycon
>>> report
<TableReport: use .open() or .markdown() to display>
```

(Note that above we only see the string representation, not the report itself,
because we are not in a notebook.)

Whether you are using a notebook or not, you can always open the report as a
full page in a separate browser tab with its `open` method:
`report.open()`.

You can also get the HTML report as a string with the `html` method or the
`html_snippet` method.
For a full, standalone web page:

```pycon
>>> report.html()
'<!DOCTYPE html>\n<html lang="en-US">\n\n<head>\n    <meta charset="utf-8"...'
```

For an HTML fragment that can be inserted into a page:

```pycon
>>> report.html_snippet()
'\n<div id="report_...-wrapper" hidden>\n    <template id="report_...'
```

If you want a summary of the report in plain-text format, you can use the
`markdown` method to get a Markdown string that can be rendered in the
notebook or used in Markdown documents. The string includes the summary
statistics for all columns, so it can be quite long for
dataframes with many columns.

```pycon
>>> md = report.markdown()
>>> print(md)
# DataFrame Report...
```

The report can also be obtained in JSON format with [`json()`](#skrub.TableReport.json), which can
be useful for programmatic access to the report data. The schema of the
JSON data is reported in [TableReport JSON schema](../table_report_json_schemahtml.md#table-report-json-schema).

Note that the resulting JSON includes the plots in SVG format, which can be
quite verbose: plots can be disabled by setting `plot_distributions=False`
when generating the report:

```pycon
>>> j = TableReport(df, plot_distributions=False).json()
>>> print(j)
{"dataframe_module": "pandas", "n_rows": 2, "n_columns": 3, "columns": ...
```

Advanced configuration: you can add custom column filters that will appear
in the report’s dropdown menu, allowing you to select a subset of columns to
display in the report.

```pycon
>>> filters = {
...         "my_filter": ["a", "b"],
... }
>>> report = TableReport(df, column_filters=filters)
```

With the code above, in addition to the default filters such as “All
columns”, “Numeric columns”, etc., the added “my_filter” will be available
in the report, selecting both columns “a” and “b”.
Filters may be specified as a list of column names, a list of column indices,
or one of the [skrub selectors](../../modules/multi_column_operations/selectorshtml.md#user-guide-selectors) objects.

### Methods

| [`dict`](#skrub.TableReport.dict)()                 | Get the report data in Python Dictionary format.                   |
|-----------------------------------------------------|--------------------------------------------------------------------|
| [`html`](#skrub.TableReport.html)()                 | Get the report as a full HTML page.                                |
| [`html_snippet`](#skrub.TableReport.html_snippet)() | Get the report as an HTML fragment that can be inserted in a page. |
| [`json`](#skrub.TableReport.json)()                 | Get the report data in JSON format.                                |
| [`markdown`](#skrub.TableReport.markdown)()         | Get the report as a Markdown string.                               |
| [`open`](#skrub.TableReport.open)()                 | Open the HTML report in a web browser.                             |
| [`write_html`](#skrub.TableReport.write_html)(file) | Store the report into an HTML file.                                |
<!-- !! processed by numpydoc !! -->

#### dict()

Get the report data in Python Dictionary format.

* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : The report data

<!-- !! processed by numpydoc !! -->

#### html()

Get the report as a full HTML page.

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The HTML page.

<!-- !! processed by numpydoc !! -->

#### html_snippet()

Get the report as an HTML fragment that can be inserted in a page.

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The HTML snippet.

<!-- !! processed by numpydoc !! -->

#### json()

Get the report data in JSON format.

By default, the JSON output includes the plots in SVG format, which can
be quite verbose. Plots can be disabled by setting
`plot_distributions=False` when generating the report.

The schema of the JSON data is reported in [TableReport JSON schema](../table_report_json_schemahtml.md#table-report-json-schema).

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The JSON data.

<!-- !! processed by numpydoc !! -->

#### markdown()

Get the report as a Markdown string.

This can be useful for displaying the report in environments that support
Markdown for formatted text, to include the report in Markdown documents,
or to get a quick text summary of the report.

#### WARNING
The Markdown output can be provided to AI agents, but it does **not**
perform any truncation or sanitization of the data. Therefore, it should
not be used with untrusted data or in contexts where the data may be too
large, as it could lead to performance issues or security risks.

* **Returns:**
  [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The Markdown report.

<!-- !! processed by numpydoc !! -->

#### open()

Open the HTML report in a web browser.

<!-- !! processed by numpydoc !! -->

#### write_html(file)

Store the report into an HTML file.

* **Parameters:**
  **file**
  : The file object or path of the file to store the HTML output.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div>
<!-- thumbnail-parent-div-close --></div>
