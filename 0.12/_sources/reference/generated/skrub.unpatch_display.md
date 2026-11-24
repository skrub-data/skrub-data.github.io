# unpatch_display

### skrub.unpatch_display(pandas=True, polars=True)

Undo the effect of `skrub.patch_display()`.

This function restores the default HTML displays of pandas and polars
DataFrames.

* **Parameters:**
  **pandas**
  : If False, do not restore the displays for pandas dataframes.

  **polars**
  : If False, do not restore the displays for polars dataframes.

#### SEE ALSO
[`patch_display`](skrub.patch_displayhtml.md#skrub.patch_display)
: Replace the default dataframe display with a TableReport.

[`TableReport`](skrub.TableReporthtml.md#skrub.TableReport)
: Directly create a report from a dataframe.

<!-- !! processed by numpydoc !! -->
