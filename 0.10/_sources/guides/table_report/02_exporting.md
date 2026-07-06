<a id="user-guide-table-report-sharing"></a>

# How to export and share the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport) for use by other tools

The [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport) is generated as a standalone HTML file that includes the report
data, the plots, and the Javascript necessary to provide interactivity.

If it is generated inside a notebook (Jupyter or Marimo), the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport) is
rendered directly inside the cell where it is called. If, instead, it is generated
by a script, the report will need to be opened by calling `.open()`:

```pycon
>>> TableReport(df).open()
```

Note that calling `.open()` will start a standalone process that hosts the report,
and a tab will be opened in the default browser. It is not possible to save the
report from the webpage. The function [`write_html()`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport.write_html) should
be used for that:

```default
tr = TableReport(df)
tr.write_html("my_report.html")
```

It is also possible to export the raw HTML, or a HTML fragment to embed in a page
with [`html()`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport.html) and  [`html_snippet()`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport.html_snippet)
respectively.

The report can be exported in JSON format, which allows structured
access to the data and statistics used to build the report with
[`json()`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport.json). The schema of the JSON data is reported in
[TableReport JSON schema](../../reference/table_report_json_schemahtml.md#table-report-json-schema).

```default
tr = TableReport(df)
json_data = tr.json()
```

Note that this will export all parts of the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport), including the distribution
plots in SVG format if they have been generated. If you do not need them, plots should be
disabled directly when generating the table report.

```default
tr = TableReport(df, plot_distributions=False)
json_data = tr.json()
```

Finally, [`markdown()`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport.markdown) produces a shortened summary of the
report in Markdown format. This summary contains the measured statistics and the
associations (if measured): plots and table preview are skipped from this view.
This format can be shared easily in text form, or fed to an AI agent to obtain
insight about a given table.

#### WARNING
No sanitization of the input data is performed, and the report includes raw data
(column names and cell values). Therefore, it should not be used on untrusted data,
or when the resulting summary may be too large as it could lead to security risks
or performance problems.
