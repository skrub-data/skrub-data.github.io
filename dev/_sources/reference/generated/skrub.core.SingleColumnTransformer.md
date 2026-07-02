# SingleColumnTransformer

### *class* skrub.core.SingleColumnTransformer

Base class for single-column transformers.

Such transformers are applied independently to each column by
[`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols); see the docstring of [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols)
for more information.

Single-column transformers are not required to inherit from this class in
order to work with [`ApplyToCols`](skrub.ApplyToColshtml.md#skrub.ApplyToCols), however doing so avoids some
boilerplate:

- The required `__single_column_transformer__` attribute is set.
- `fit` is defined (calls `fit_transform` and discards the result).
- `fit`, `transform` and `fit_transform` are wrapped to check
  : that the input is a single column and raise a `ValueError` with a
    helpful message when it is not.
- A note about single-column transformers (vs dataframe transformers)
  : is added after the summary line of the docstring.

Subclasses must define `fit_transform` and `transform` (or inherit them
from another superclass).

### Methods

| [`fit`](#skrub.core.SingleColumnTransformer.fit)(column[, y])                                          | Fit the transformer.                         |
|--------------------------------------------------------------------------------------------------------|----------------------------------------------|
| [`get_feature_names_out`](#skrub.core.SingleColumnTransformer.get_feature_names_out)([input_features]) | Get the output feature names.                |
| [`get_params`](#skrub.core.SingleColumnTransformer.get_params)([deep])                                 | Get parameters for this estimator.           |
| [`set_output`](#skrub.core.SingleColumnTransformer.set_output)(\*[, transform])                        | Default no-op implementation for set_output. |
| [`set_params`](#skrub.core.SingleColumnTransformer.set_params)(\*\*params)                             | Set the parameters of this estimator.        |
<!-- !! processed by numpydoc !! -->

#### fit(column, y=None, \*\*kwargs)

Fit the transformer.

This default implementation simply calls `fit_transform()` and
returns `self`.

Subclasses should implement `fit_transform` and `transform`.

* **Parameters:**
  **column**
  : Unlike most scikit-learn transformers, single-column transformers
    transform a single column, not a whole dataframe.

  **y**
  : Prediction targets.

  **\*\*kwargs**
  : Extra named arguments are passed to `self.fit_transform()`.
* **Returns:**
  self
  : The fitted transformer.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Get the output feature names.

* **Parameters:**
  **input_features**
  : Input feature names. Ignored.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The names of the output features.

<!-- !! processed by numpydoc !! -->

#### get_params(deep=True)

Get parameters for this estimator.

* **Parameters:**
  **deep**
  : If True, will return the parameters for this estimator and
    contained subobjects that are estimators.
* **Returns:**
  **params**
  : Parameter names mapped to their values.

<!-- !! processed by numpydoc !! -->

#### set_output(, transform=None)

Default no-op implementation for set_output.

Skrub transformers already output dataframes of the correct type by
default so there is usually no need for set_output to do anything.

Subclasses are of course free to redefine set_output (e.g. by
inheriting from `TransformerMixin` before SingleColumnTransformer).

* **Parameters:**
  **transform**
  : Ignored.
* **Returns:**
  SingleColumnTransformer
  : Returns self.

<!-- !! processed by numpydoc !! -->

#### set_params(\*\*params)

Set the parameters of this estimator.

The method works on simple estimators as well as on nested objects
(such as [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)). The latter have
parameters of the form `<component>__<parameter>` so that it’s
possible to update each component of a nested object.

* **Parameters:**
  **\*\*params**
  : Estimator parameters.
* **Returns:**
  **self**
  : Estimator instance.

<!-- !! processed by numpydoc !! -->
