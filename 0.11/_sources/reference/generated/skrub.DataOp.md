# DataOp

### *class* skrub.DataOp(impl)

Representation of a computation that can be used to build DataOps plans and learners.

A complete machine learning pipeline – from data loading and wrangling to the final
prediction – in a single object that can be fitted, tuned, cross-validated, and
saved like any scikit-learn estimator.

This class is usually not instantiated manually, but through one of the functions
[`var()`](skrub.var.md#skrub.var), [`as_data_op()`](skrub.as_data_op.md#skrub.as_data_op), [`X()`](skrub.X.md#skrub.X) or [`y()`](skrub.y.md#skrub.y), by applying a
[`deferred()`](skrub.deferred.md#skrub.deferred) function, or by calling a method or applying an operator
to an existing DataOp.

Refer to the [Building complete pipelines with DataOps](../../data_ops.md#user-guide-data-ops-index) page for more information.

<!-- !! processed by numpydoc !! -->
