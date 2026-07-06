<a id="user-guide-table-report-customize"></a>

# How to tweak the Appearance of the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport)

The skrub global configuration includes various parameters that let you tweak
the HTML representation of the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport).

For performance reasons, the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport) disables the computation of
distributions and associations for tables with more than 30 columns.
This behavior can be overridden by setting the parameters `plot_distributions`
and `compute_associations` to `True` respectively.

It is also possible to specify the floating point precision by setting the appropriate
`float_precision` parameter.

The column threshold that is used by the [`TableReport`](../../reference/generated/skrub.TableReporthtml.md#skrub.TableReport) can be modified in a given
script by using [`set_config()`](../../reference/generated/skrub.set_confightml.md#skrub.set_config) and changing the values of
`table_report_plot_threshold` and `table_report_associations_threshold` to
the desired threshold. Environment variables are also provided to set the threshold
permanently. Refer to [How to configure and customize the default behavior of skrub](../utilities/customizing_configurationhtml.md#user-guide-configuration-parameters) for more detail.
