# DataOp

### *class* skrub.DataOp(impl)

Representation of a computation that can be used to build DataOps plans and learners.

A complete machine learning pipeline – from data loading and wrangling to the final
prediction – in a single object that can be fitted, tuned, cross-validated, and
saved like any scikit-learn estimator.

This class is usually not instantiated manually, but through one of the functions
[`var()`](skrub.varhtml.md#skrub.var), [`as_data_op()`](skrub.as_data_ophtml.md#skrub.as_data_op), [`X()`](skrub.Xhtml.md#skrub.X) or [`y()`](skrub.yhtml.md#skrub.y), by applying a
[`deferred()`](skrub.deferredhtml.md#skrub.deferred) function, or by calling a method or applying an operator
to an existing DataOp.

Refer to the [Building complete pipelines with DataOps](../../data_opshtml.md#user-guide-data-ops-index) page for more information.

<!-- !! processed by numpydoc !! -->
