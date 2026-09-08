# DataOp

### *class* skrub.DataOp(impl)

Representation of a computation that can be used to build DataOps plans and learners.

Please refer to the example gallery for an introduction to skrub
DataOps.

This class is usually not instantiated manually, but through one of the functions
[`var()`](skrub.var.md#skrub.var), [`as_data_op()`](skrub.as_data_op.md#skrub.as_data_op), [`X()`](skrub.X.md#skrub.X) or [`y()`](skrub.y.md#skrub.y), by applying a
[`deferred()`](skrub.deferred.md#skrub.deferred) function, or by calling a method or applying an operator
to an existing DataOp.

<!-- !! processed by numpydoc !! -->
