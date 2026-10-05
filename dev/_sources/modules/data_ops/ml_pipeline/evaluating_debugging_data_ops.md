<a id="user-guide-data-ops-evaluating-debugging-dataops"></a>

# Evaluating and debugging the DataOps plan with [`.skb.report()`](../../../reference/generated/skrub.DataOp.skb.report.md#skrub.DataOp.skb.report)

All operations on DataOps are recorded in a computational graph, which can be
inspected with [`.skb.report()`](../../../reference/generated/skrub.DataOp.skb.report.md#skrub.DataOp.skb.report). This method
generates an HTML report that shows the full plan, including all nodes, their names,
descriptions, and the transformations applied to the data. It is possible to give a
title to the evaluation report this way:
`my_data_op.skb.report(title="my title")`.

An example of the report can be found
[here](../../../_static/credit_fraud_report/index.html).

For each node in the plan, the report shows:

- The name and the description of the node, if present.
- Predecessor and successor nodes in the computational graph.
- Where the code relative to the node is defined.
- The estimator fitted in the node along with its parameters (if applicable).
- The preview of the data at that node.

Additionally, if computations fail in the plan, the report shows the offending
node and the error message, which can help in debugging the plan.

It is possible to pass `eval=False` to generate a report without actually
evaluating the DataOp, without running any computations. In this case the
information that is available without running anything (e.g. some applied
function and estimator names and docstrings) is shown, but no outputs are
displayed.

By default, reports are saved in the `skrub_data/execution_reports` directory, but
they can be saved to a different location with the `output_dir` parameter.
Note that the default path can be altered with the
`SKB_DATA_DIRECTORY` environment variable. See [How to configure and customize the default behavior of skrub](../../../guides/utilities/customizing_configuration.md#user-guide-configuration-parameters)
for more details.
