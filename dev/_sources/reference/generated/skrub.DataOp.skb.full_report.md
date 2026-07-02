# skrub.DataOp.skb.full_report

#### DataOp.skb.full_report(environment=None, open=True, output_dir=None, overwrite=False, title=None)

Generate a full report of the DataOp’s evaluation.

This creates a report showing the computation graph, and for each
intermediate computation, some information (the line of code where it
was defined, the time it took to run, and more) and a display of the
intermediate result (or error). By default, the report is stored in
a timestamped subdirectory of the skrub data folder.

#### NOTE
When this function is invoked reports starting with `full_data_op_report_`
that are stored in the skrub data folder are automatically deleted after
7 days.
This is to avoid accumulating too many reports over time. If you want
to keep specific reports, please specify an output directory.

* **Parameters:**
  **environment**
  : Bindings for variables and choices contained in the DataOp. If
    not provided, the variables’ `value` and the choices default
    value are used.

  **open**
  : Whether to open the report in a web browser once computed.

  **output_dir**
  : Directory where to store the report. If `None`, a timestamped
    subdirectory will be created in the skrub data directory. Note
    that the reports created with `output_dir=None` are automatically
    deleted after 7 days.

  **overwrite**
  : What to do if the output directory already exists. If
    `overwrite`, replace it, otherwise raise an exception.

  **title: str (default=None)**
  : Title to display at the top of the report. If `None`, no title will be
    displayed.
* **Returns:**
  [`dict`](https://docs.python.org/3/library/stdtypes.html#dict)
  : The results of evaluating the DataOp. The keys are
    `'result'`, `'error'` and `'report_path'`. If the execution
    raised an exception, it is contained in `'error'` and
    `'result'` is `None`. Otherwise the result produced by the
    evaluation is in `'result'` and `'error'` is `None`. Either
    way a report is stored at the location indicated by
    `'report_path'`.

#### SEE ALSO
[`SkrubLearner.report()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.report)
: Generate a report for a call to any of the methods of the [`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) such as `transform()`, `predict()`, `predict_proba()` etc.

### Notes

The learner is run doing a `fit_transform`. To get a report for other
methods (e.g. `predict`, see [`SkrubLearner.report()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.report)). If
`environment` is provided, it is used as the bindings for the
variables in the DataOp, and otherwise, the `value` attributes of the
variables are used.

At the moment, this creates a directory on the filesystem containing
HTML files. The report can be displayed by visiting the contained
`index.html` in a webbrowser, or passing `open=True` (the default)
to this method.

### Examples

```pycon
>>> # ignore this line:
>>> import pytest; pytest.skip('graphviz may not be installed')
```

```pycon
>>> import skrub
>>> c = skrub.var('a', 1) / skrub.var('b', 2)
>>> report = c.skb.full_report(open=False)
>>> report['result']
0.5
>>> report['error']
>>> report['report_path']
PosixPath('.../skrub_data/execution_reports/full_data_op_report_.../index.html')
```

We pass data:

```pycon
>>> report = c.skb.full_report({'a': 33, 'b': 11 }, open=False)
>>> report['result']
3.0
```

And if there was an error:

```pycon
>>> report = c.skb.full_report({'a': 1, 'b': 0}, open=False)
>>> report['result']
>>> report['error']
ZeroDivisionError('division by zero')
>>> report['report_path']
PosixPath('.../skrub_data/execution_reports/full_data_op_report_.../index.html')
```

<!-- !! processed by numpydoc !! -->
