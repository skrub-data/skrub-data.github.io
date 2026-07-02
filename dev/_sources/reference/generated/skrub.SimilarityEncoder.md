# SimilarityEncoder

### *class* skrub.SimilarityEncoder(\*, ngram_range=(2, 4), analyzer='char', categories='auto', dtype=<class 'numpy.float64'>, handle_unknown='ignore', handle_missing='', hashing_dim=None, n_jobs=None)

Encode string categories to a similarity matrix, to capture fuzziness across a few categories.

The input to this transformer should be an array-like of strings.
The method is based on calculating the morphological similarities
between the categories.
This encoding is an alternative to OneHotEncoder for
dirty categorical variables.

The principle of this encoder is as follows:

1. Given an input string array `X = [x1, ..., xn]` with `k` unique
   categories `[c1, ..., ck]` and a similarity measure `sim(s1, s2)`
   between strings, we define the encoded vector of `xi` as
   `[sim(xi, c1), ... , sim(xi, ck)]`.
   Similarity encoding of `X` results in a matrix with shape (`n`, `k`)
   that captures morphological similarities between string entries.
2. To avoid dealing with high-dimensional encodings when `k` is high,
   we can use `d << k` prototypes `[p1, ..., pd]` with which
   similarities will be computed:  `xi -> [sim(xi, p1), ..., sim(xi, pd)]`.
   These prototypes can be provided by the user. Otherwise, we recommend
   using the MinHashEncoder or GapEncoder when taking all unique entries
   leads to too many prototypes.

The similarity measure is based on the proportion of common n-grams between
two strings.

* **Parameters:**
  **ngram_range**
  : The lower and upper boundaries of the range of n-values for different
    n-grams used in the string similarity. All values of `n` such
    that `min_n <= n <= max_n` will be used.

  **analyzer**
  : Analyzer parameter for the HashingVectorizer / CountVectorizer.
    Describes whether the matrix `V` to factorize should be made of
    word counts or character-level n-gram counts.
    Option ‘char_wb’ creates character n-grams only from text inside word
    boundaries; n-grams at the edges of words are padded with space.

  **categories**
  : Categories (unique values) per feature:
    - ‘auto’ : Determine categories automatically from the training data.
    - list : `categories[i]` holds the categories expected in the i-th
      column. The passed categories must be sorted and should not mix
      strings and numeric values.
    <br/>
    The categories used can be found in the `categories_`
    attribute.

  **dtype**
  : Desired dtype of output.

  **handle_unknown**
  : Whether to raise an error or ignore if an unknown categorical feature
    is present during transform (default is to ignore). When this parameter
    is set to ‘ignore’ and an unknown category is encountered during
    transform, the resulting encoded columns for this feature
    will be all zeros. In the inverse transform, an unknown category
    will be denoted as None.

  **handle_missing**
  : Whether to raise an error or impute with blank string ‘’ if missing
    values (NaN) are present during fit (default is to impute).
    When this parameter is set to ‘’, and a missing value is encountered
    during fit_transform, the resulting encoded columns for this feature
    will be all zeros. In the inverse transform, the missing category
    will be denoted as None.
    “Missing values” are any value for which `pandas.isna` returns
    `True`, such as `numpy.nan` or `None`.

  **hashing_dim**
  : If `None`, the base vectorizer is a CountVectorizer, otherwise it is a
    HashingVectorizer with a number of features equal to `hashing_dim`.

  **n_jobs**
  : Maximum number of processes used to compute similarity matrices. Used
    only if `fast=True` in SimilarityEncoder.transform.
* **Attributes:**
  **categories_**
  : The categories of each feature determined during fitting
    (in the same order as the output of SimilarityEncoder.transform).

#### SEE ALSO
[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
: Encode string columns as a numeric array with the minhash method.

[`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder)
: Encodes dirty categories (strings) by constructing latent topics with continuous encoding.

[`TextEncoder`](skrub.TextEncoderhtml.md#skrub.TextEncoder)
: Encode string columns with a pretrained language model.

[`deduplicate`](skrub.deduplicatehtml.md#skrub.deduplicate)
: Deduplicate data by hierarchically clustering similar strings.

### Notes

The functionality of SimilarityEncoder is easy to explain and understand,
but it is not scalable. It is useful only to capture links across a few categories
(eg eg: “west”, “north”, “north-west”), but not when there are many categories,
as with open-ended entries.
Instead, the GapEncoder is usually recommended.

### References

For a detailed description of the method, see
[Similarity encoding for learning with dirty categorical variables](https://hal.inria.fr/hal-01806175) by Cerda, Varoquaux, Kegl. 2018
(Machine Learning journal, Springer).

### Examples

```pycon
>>> from skrub import SimilarityEncoder
>>> enc = SimilarityEncoder()
>>> X = [['Male', 1], ['Female', 3], ['Female', 2]]
>>> enc.fit(X)
SimilarityEncoder()
```

It inherits the same methods as theOneHotEncoder:

```pycon
>>> enc.categories_
[array(['Female', 'Male'], dtype=object), array([1, 2, 3], dtype=object)]
```

But it provides a continuous encoding based on similarity
instead of a discrete one based on exact matches:

```pycon
>>> enc.transform([['Female', 1], ['Male', 4]])
array([[1.        , 0.42..., 1.        , 0.        , 0.        ],
       [0.42..., 1.        , 0.        , 0.        , 0.        ]])
>>> enc.get_feature_names_out(['gender', 'group'])
array(['gender_Female', 'gender_Male', 'group_1', 'group_2', 'group_3'],
      dtype=object)
```

### Methods

| [`fit`](#skrub.SimilarityEncoder.fit)(X[, y])                                               | Fit the instance to `X`.                                                               |
|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.SimilarityEncoder.fit_transform)(X[, y])                           | Fit to data, then transform it.                                                        |
| [`get_feature_names_out`](#skrub.SimilarityEncoder.get_feature_names_out)([input_features]) | Get output feature names for transformation.                                           |
| [`get_params`](#skrub.SimilarityEncoder.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`inverse_transform`](#skrub.SimilarityEncoder.inverse_transform)(X)                        | Convert the data back to the original representation.                                  |
| [`set_output`](#skrub.SimilarityEncoder.set_output)(\*[, transform])                        | Set output container.                                                                  |
| [`set_params`](#skrub.SimilarityEncoder.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.SimilarityEncoder.set_transform_request)(\*[, fast])       | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.SimilarityEncoder.transform)(X[, fast])                                | Transform `X` using specified encoding scheme.                                         |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit the instance to `X`.

* **Parameters:**
  **X**
  : The data to determine the categories of each feature.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`SimilarityEncoder`](#skrub.SimilarityEncoder)
  : The fitted SimilarityEncoder instance (self).

<!-- !! processed by numpydoc !! -->

#### fit_transform(X, y=None, \*\*fit_params)

Fit to data, then transform it.

Fits transformer to `X` and `y` with optional parameters `fit_params`
and returns a transformed version of `X`.

* **Parameters:**
  **X**
  : Input samples.

  **y**
  : Target values (None for unsupervised transformations).

  **\*\*fit_params**
  : Additional fit parameters.
    Pass only if the estimator accepts additional params in its `fit` method.
* **Returns:**
  **X_new**
  : Transformed array.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Get output feature names for transformation.

* **Parameters:**
  **input_features**
  : Input features.
    - If `input_features` is `None`, then `feature_names_in_` is
      used as feature names in. If `feature_names_in_` is not defined,
      then the following input feature names are generated:
      `["x0", "x1", ..., "x(n_features_in_ - 1)"]`.
    - If `input_features` is an array-like, then `input_features` must
      match `feature_names_in_` if `feature_names_in_` is defined.
* **Returns:**
  **feature_names_out**
  : Transformed feature names.

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

#### *property* infrequent_categories_

Infrequent categories for each feature.

<!-- !! processed by numpydoc !! -->

#### inverse_transform(X)

Convert the data back to the original representation.

When unknown categories are encountered (all zeros in the
one-hot encoding), `None` is used to represent this category. If the
feature with the unknown category has a dropped category, the dropped
category will be its inverse.

For a given input feature, if there is an infrequent category,
‘infrequent_sklearn’ will be used to represent the infrequent category.

* **Parameters:**
  **X**
  : The transformed data.
* **Returns:**
  **X_original**
  : Inverse transformed array.

<!-- !! processed by numpydoc !! -->

#### set_output(, transform=None)

Set output container.

Refer to the [user guide](https://scikit-learn.org/stable/modules/df_output_transform.html#df-output-transform) for more details
and [Introducing the set_output API](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_set_output.html#sphx-glr-auto-examples-miscellaneous-plot-set-output-py) for an
example on how to use the API.

* **Parameters:**
  **transform**
  : Configure output of `transform` and `fit_transform`.
    - `"default"`: Default output format of a transformer
    - `"pandas"`: DataFrame output
    - `"polars"`: Polars output
    - `None`: Transform configuration is unchanged
    <br/>
    #### Versionadded
    Added in version 1.4: `"polars"` option was added.
* **Returns:**
  **self**
  : Estimator instance.

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

#### set_transform_request(, fast='$UNCHANGED$')

Configure whether metadata should be requested to be passed to the `transform` method.

Note that this method is only relevant when this estimator is used as a
sub-estimator within a [meta-estimator](https://scikit-learn.org/stable/glossary.html#term-meta-estimator) and metadata routing is enabled
with `enable_metadata_routing=True` (see [`sklearn.set_config()`](https://scikit-learn.org/stable/modules/generated/sklearn.set_config.html#sklearn.set_config)).
Please check the [User Guide](https://scikit-learn.org/stable/metadata_routing.html#metadata-routing) on how the routing
mechanism works.

The options for each parameter are:

- `True`: metadata is requested, and passed to `transform` if provided. The request is ignored if metadata is not provided.
- `False`: metadata is not requested and the meta-estimator will not pass it to `transform`.
- `None`: metadata is not requested, and the meta-estimator will raise an error if the user provides it.
- `str`: metadata should be passed to the meta-estimator with this given alias instead of the original name.

The default (`sklearn.utils.metadata_routing.UNCHANGED`) retains the
existing request. This allows you to change the request for some
parameters and not others.

#### Versionadded
Added in version 1.3.

* **Parameters:**
  **fast**
  : Metadata routing for `fast` parameter in `transform`.
* **Returns:**
  **self**
  : The updated object.

<!-- !! processed by numpydoc !! -->

#### transform(X, fast=True)

Transform `X` using specified encoding scheme.

* **Parameters:**
  **X**
  : The data to encode.

  **fast**
  : Whether to use the fast computation of ngrams.
* **Returns:**
  [`ndarray`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.html#numpy.ndarray), shape [n_samples, n_features_new]
  : Transformed input.

<!-- !! processed by numpydoc !! -->
