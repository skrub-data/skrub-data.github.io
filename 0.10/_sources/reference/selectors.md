<a id="selectors-ref"></a>

# Selectors

Contains method to select columns in a dataframe. See the [selectors](../modules/multi_column_operations/selectorshtml.md#user-guide-selectors) section for further details.

| [`selectors.Selector`](generated/skrub.selectors.Selectorhtml.md#skrub.selectors.Selector)                            | Generic selector type, that returns set columns when applied.                                                    |
|-----------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| [`selectors.all`](generated/skrub.selectors.allhtml.md#skrub.selectors.all)                                           | Select all columns.                                                                                              |
| [`selectors.any_date`](generated/skrub.selectors.any_datehtml.md#skrub.selectors.any_date)                            | Select columns that have a Date or Datetime data type.                                                           |
| [`selectors.boolean`](generated/skrub.selectors.booleanhtml.md#skrub.selectors.boolean)                               | Select columns that have a Boolean data type.                                                                    |
| [`selectors.cardinality_below`](generated/skrub.selectors.cardinality_belowhtml.md#skrub.selectors.cardinality_below) | Select columns whose cardinality (number of unique values) is (strictly)     below `threshold`.                  |
| [`selectors.categorical`](generated/skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)                   | Select columns that have a Categorical (or polars Enum) data type.                                               |
| [`selectors.cols`](generated/skrub.selectors.colshtml.md#skrub.selectors.cols)                                        | Select columns by name.                                                                                          |
| [`selectors.filter`](generated/skrub.selectors.filterhtml.md#skrub.selectors.filter)                                  | Select columns for which `predicate` returns True.                                                               |
| [`selectors.filter_names`](generated/skrub.selectors.filter_nameshtml.md#skrub.selectors.filter_names)                | Select columns based on their name.                                                                              |
| [`selectors.float`](generated/skrub.selectors.floathtml.md#skrub.selectors.float)                                     | Select columns that have a floating-point data type.                                                             |
| [`selectors.glob`](generated/skrub.selectors.globhtml.md#skrub.selectors.glob)                                        | Select columns by name with Unix shell style 'glob' pattern.                                                     |
| [`selectors.has_dtype`](generated/skrub.selectors.has_dtypehtml.md#skrub.selectors.has_dtype)                         | Select columns whose dtype is equal to one of the provided dtypes.                                               |
| [`selectors.has_nulls`](generated/skrub.selectors.has_nullshtml.md#skrub.selectors.has_nulls)                         | Select columns that contain at least one null value, or a proportion of null     values above a given threshold. |
| [`selectors.integer`](generated/skrub.selectors.integerhtml.md#skrub.selectors.integer)                               | Select columns that have an integer data type.                                                                   |
| [`selectors.inv`](generated/skrub.selectors.invhtml.md#skrub.selectors.inv)                                           | Invert a selector.                                                                                               |
| [`selectors.make_selector`](generated/skrub.selectors.make_selectorhtml.md#skrub.selectors.make_selector)             | Transform a selector, column name or list of column names into a selector.                                       |
| [`selectors.numeric`](generated/skrub.selectors.numerichtml.md#skrub.selectors.numeric)                               | Select columns that have a numeric data type.                                                                    |
| [`selectors.object`](generated/skrub.selectors.objecthtml.md#skrub.selectors.object)                                  | Select columns whose dtype is `object` (pandas) or `pl.Object` (polars).                                         |
| [`selectors.regex`](generated/skrub.selectors.regexhtml.md#skrub.selectors.regex)                                     | Select columns by name with a regular expression.                                                                |
| [`selectors.select`](generated/skrub.selectors.selecthtml.md#skrub.selectors.select)                                  | Apply a selector to a dataframe and return a dataframe with the selected     columns.                            |
| [`selectors.string`](generated/skrub.selectors.stringhtml.md#skrub.selectors.string)                                  | Select columns that have a String data type.                                                                     |
