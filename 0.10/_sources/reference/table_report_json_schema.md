<a id="table-report-json-schema"></a>

# TableReport JSON schema

[`TableReport.json()`](generated/skrub.TableReporthtml.md#skrub.TableReport.json) returns a JSON string whose
top-level object contains the keys described below.

## Top-level object

| Key                     | Type                                                            | Description                                                                                                                                                                                                                                                            |
|-------------------------|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `dataframe_module`      | string                                                          | Name of the dataframe library used. Either `"pandas"` or<br/>`"polars"`.                                                                                                                                                                                               |
| `n_rows`                | integer                                                         | Number of rows in the dataframe.                                                                                                                                                                                                                                       |
| `n_columns`             | integer                                                         | Number of columns in the dataframe.                                                                                                                                                                                                                                    |
| `dataframe_is_empty`    | boolean                                                         | `true` when the dataframe has no rows or no columns.                                                                                                                                                                                                                   |
| `plots_skipped`         | boolean                                                         | `true` when `plot_distributions=False` was passed to<br/>[`TableReport`](generated/skrub.TableReporthtml.md#skrub.TableReport). When `true`, plot keys are absent from<br/>column objects, but `histogram_data` is still present for numeric and<br/>datetime columns. |
| `associations_skipped`  | boolean                                                         | `true` when association computation was skipped (either because<br/>`with_associations=False` was passed, or because polars was used<br/>without pyarrow installed).                                                                                                   |
| `cardinality_threshold` | integer                                                         | The threshold from [`get_config()`](generated/skrub.get_confightml.md#skrub.get_config) above which a column is<br/>considered high-cardinality. Default: 40.                                                                                                          |
| `n_constant_columns`    | integer                                                         | Number of columns whose values are all identical.                                                                                                                                                                                                                      |
| `columns`               | array of [column objects](#table-report-json-schema-column)     | One entry per column, in the original column order.                                                                                                                                                                                                                    |
| `top_associations`      | array of [association objects](#table-report-json-schema-assoc) | Column-pair association scores (up to 1 000 pairs, sorted by strength).<br/>Present only when `associations_skipped` is `false`.                                                                                                                                       |
| `title`                 | string                                                          | **Optional.** Present only when a `title` argument was passed to<br/>[`TableReport`](generated/skrub.TableReporthtml.md#skrub.TableReport).                                                                                                                            |
| `order_by`              | string                                                          | **Optional.** Name of the column used for sorting when the deprecated<br/>parameter `order_by` was passed to [`TableReport`](generated/skrub.TableReporthtml.md#skrub.TableReport).                                                                                    |

<a id="table-report-json-schema-column"></a>

## Column object

Every entry in `columns` contains the following keys. Additional keys are
present depending on the column’s dtype; they are documented in the subsections
below.

| Key                 | Type             | Description                                                                                                                                                                                    |
|---------------------|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `position`          | integer          | Zero-based index of the column in the dataframe (same as `idx`).                                                                                                                               |
| `idx`               | integer          | Zero-based index of the column in the dataframe (same as<br/>`position`).                                                                                                                      |
| `name`              | string           | Column name.                                                                                                                                                                                   |
| `dtype`             | string           | Column dtype as a string (e.g. `"float64"`, `"object"`,<br/>`"datetime64[ns]"`).                                                                                                               |
| `null_count`        | integer          | Number of null / NaN values.                                                                                                                                                                   |
| `null_proportion`   | number           | Fraction of null values in `[0, 1]`.                                                                                                                                                           |
| `nulls_level`       | string           | Summary severity level: `"ok"` (no nulls), `"warning"` (some<br/>nulls), or `"critical"` (all values are null).                                                                                |
| `value_is_constant` | boolean          | `true` when every non-null value in the column is identical.                                                                                                                                   |
| `is_ordered`        | boolean          | `true` when the column values are sorted in ascending or descending<br/>order.                                                                                                                 |
| `plot_names`        | array of strings | Names of the plot keys that are present on this column object (e.g.<br/>`["histogram_plot"]`). Empty when `plots_skipped` is `true` or<br/>when the column is constant or contains only nulls. |

### Columns containing only nulls

When `null_count` equals `n_rows` (all values are null) only the keys of
the base column object above are present; no statistical keys are added.

### Categorical / string columns

Present for non-numeric, non-datetime, non-duration columns that are not
entirely null.

| Key                    | Type                            | Description                                                                                                                                                                                               |
|------------------------|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `n_unique`             | integer                         | Number of distinct values (including null).                                                                                                                                                               |
| `unique_proportion`    | number                          | Fraction of distinct values relative to `n_rows`.                                                                                                                                                         |
| `is_high_cardinality`  | boolean                         | `true` when `n_unique` exceeds `cardinality_threshold`.                                                                                                                                                   |
| `value_counts`         | array of `[value, count]` pairs | The up to 10 most frequent values and their counts, sorted by<br/>frequency descending. Each element is a two-element array<br/>`[value, count]` where `value` is a string and `count` is an<br/>integer. |
| `most_frequent_values` | array of strings                | The up to 10 most frequent values (same order as `value_counts`).                                                                                                                                         |
| `constant_value`       | any                             | **Optional**: Present only when `value_is_constant` is `true`. The single<br/>value shared by all non-null rows.                                                                                          |
| `value_counts_plot`    | string (SVG)                    | **Optional**: Present only when `plots_skipped` is `false` and<br/>`value_is_constant` is `false`. Bar chart of the top value<br/>counts as an inline SVG string.                                         |

### Numeric columns

Present for columns whose dtype is numeric (integer or float), or duration
(timedelta). Boolean columns have a subset of these keys (only `mean`).

| Key                    | Type                                                         | Description                                                                                                                                                                |
|------------------------|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `n_unique`             | integer                                                      | Number of distinct values.                                                                                                                                                 |
| `unique_proportion`    | number                                                       | Fraction of distinct values relative to `n_rows`.                                                                                                                          |
| `is_high_cardinality`  | boolean                                                      | `true` when `n_unique` exceeds `cardinality_threshold`.                                                                                                                    |
| `mean`                 | number                                                       | Arithmetic mean of non-null values.                                                                                                                                        |
| `standard_deviation`   | number                                                       | Standard deviation of non-null values. `null` when it cannot be<br/>computed (e.g. single non-null value).                                                                 |
| `inter_quartile_range` | number                                                       | Difference between the 75th and 25th percentiles.                                                                                                                          |
| `quantiles`            | object                                                       | Map of quantile level (as a numeric key) to value, for levels<br/>`0.0`, `0.25`, `0.5`, `0.75`, and `1.0`. Absent when<br/>`value_is_constant` is `true`.                  |
| `constant_value`       | number                                                       | **Optional**. Present only when `value_is_constant` is `true`. The single<br/>value shared by all non-null rows.                                                           |
| `is_duration`          | boolean                                                      | `true` for timedelta / duration columns (values were converted to a<br/>numeric unit before statistics were computed).                                                     |
| `duration_unit`        | string or null                                               | Unit used when `is_duration` is `true`: one of<br/>`"microsecond"`, `"millisecond"`, `"second"`, `"hour"`,<br/>`"day"`, or `"year"`. `null` for non-duration columns.      |
| `histogram_data`       | [histogram data object](#table-report-json-schema-hist-data) | Bin counts and edges for the distribution histogram. Always present<br/>for non-constant numeric columns (even when `plots_skipped` is<br/>`true`).                        |
| `histogram_plot`       | string (SVG)                                                 | **Optional**. Present only when `plots_skipped` is `false` and<br/>`order_by` is not set. Distribution histogram as an inline SVG<br/>string.                              |
| `line_plot`            | string (SVG)                                                 | **Optional**. Present only when `plots_skipped` is `false` and `order_by`<br/>is set. Line chart of the column values against the sort column as<br/>an inline SVG string. |

### Datetime columns

Present for columns with a date or datetime dtype.

| Key                   | Type                                                         | Description                                                                                                                                            |
|-----------------------|--------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| `n_unique`            | integer                                                      | Number of distinct values.                                                                                                                             |
| `unique_proportion`   | number                                                       | Fraction of distinct values relative to `n_rows`.                                                                                                      |
| `is_high_cardinality` | boolean                                                      | `true` when `n_unique` exceeds `cardinality_threshold`.                                                                                                |
| `min`                 | string (ISO 8601)                                            | Earliest datetime value. Absent when `value_is_constant` is<br/>`true`.                                                                                |
| `max`                 | string (ISO 8601)                                            | Latest datetime value. Absent when `value_is_constant` is<br/>`true`.                                                                                  |
| `constant_value`      | string (ISO 8601)                                            | **Optional**. Present only when `value_is_constant` is `true`. The single<br/>datetime value shared by all non-null rows.                              |
| `histogram_data`      | [histogram data object](#table-report-json-schema-hist-data) | Bin counts and edges. Always present for non-constant datetime<br/>columns (even when `plots_skipped` is `true`).                                      |
| `histogram_plot`      | string (SVG)                                                 | **Optional**. Present only when `plots_skipped` is `false` and<br/>`value_is_constant` is `false`. Distribution histogram as an<br/>inline SVG string. |

<a id="table-report-json-schema-hist-data"></a>

## Histogram data object

The `histogram_data` key on numeric and datetime column objects contains an
object with the following keys.

| Key                    | Type              | Description                                                                                                                                                                                   |
|------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `bin_counts`           | array of integers | Count of values in each bin (length *n*).                                                                                                                                                     |
| `bin_edges`            | array of numbers  | Left and right edges of each bin (length *n + 1*). Refer to<br/>[`numpy.histogram()`](https://numpy.org/doc/stable/reference/generated/numpy.histogram.html#numpy.histogram) for more detail. |
| `n_low_outliers`       | integer           | Number of values below the plotted range that were excluded from the<br/>histogram bins.                                                                                                      |
| `n_high_outliers`      | integer           | Number of values above the plotted range that were excluded from the<br/>histogram bins.                                                                                                      |
| `total_seconds_offset` | number            | Only present for datetime columns. Offset value subtracted from all values<br/>before building the histogram. The offset is the minimum of the column.                                        |

<a id="table-report-json-schema-assoc"></a>

## Association object

Each entry in the top-level `top_associations` array describes the
association between a pair of columns.

| Key                 | Type           | Description                                                                                                                                                                                               |
|---------------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `left_column_name`  | string         | Name of the first column in the pair.                                                                                                                                                                     |
| `left_column_idx`   | integer        | Zero-based index of the first column in the pair.                                                                                                                                                         |
| `right_column_name` | string         | Name of the second column in the pair.                                                                                                                                                                    |
| `right_column_idx`  | integer        | Zero-based index of the second column in the pair.                                                                                                                                                        |
| `cramer_v`          | number or null | [Cramér’s V](https://en.wikipedia.org/wiki/Cram%C3%A9r%27s_V)<br/>statistic (`[0, 1]`) for the pair. `null` when it could not be<br/>computed.                                                            |
| `pearson_corr`      | number or null | [Pearson correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)<br/>coefficient (`[-1, 1]`) for the pair. `null` when it could not be<br/>computed (e.g. one column is not numeric). |
