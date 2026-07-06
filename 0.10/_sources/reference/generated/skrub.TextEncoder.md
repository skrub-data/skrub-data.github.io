# TextEncoder

### *class* skrub.TextEncoder(model_name='intfloat/e5-small-v2', n_components=30, device=None, batch_size=32, token_env_variable=None, cache_folder=None, store_weights_in_pickle=False, random_state=None, verbose=False)

Encode string features by applying a pretrained language model         downloaded from the HuggingFace Hub.

This is a thin wrapper around `SentenceTransformer`
that follows the scikit-learn API, making it usable within a scikit-learn pipeline.

#### WARNING
To use this class, you need to install the optional `transformers`
dependencies for skrub. See the “deep learning dependencies” section
in the [Install](../../installhtml.md#installation-instructions) guide for more details.

* **Parameters:**
  **model_name**
  : - If a filepath on disk is passed, this class loads the model from that path.
    - Otherwise, it first tries to download a pre-trained
      `SentenceTransformer` model.
      If that fails, tries to construct a model from Huggingface models repository
      with that name.
    <br/>
    The following models have a good performance/memory usage tradeoff:
    - `intfloat/e5-small-v2`
    - `all-MiniLM-L6-v2`
    - `all-mpnet-base-v2`
    <br/>
    You can find more options on the [sentence-transformers documentation](https://www.sbert.net/docs/pretrained_models.html#model-overview).
    <br/>
    The default model is a shrunk version of e5-v2, which has shown good
    performance in the benchmark of [[1]](#rdcc4582c1cac-1).

  **n_components**
  : The number of embedding dimensions. As the number of dimensions is different
    across embedding models, this class uses a [`PCA`](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html#sklearn.decomposition.PCA)
    to set the number of embedding to `n_components` during `transform`.
    Set `n_components=None` to skip the PCA dimension reduction mechanism.
    <br/>
    See [[1]](#rdcc4582c1cac-1) for more details on the choice of the PCA and default
    `n_components`.

  **device**
  : Device (e.g. “cpu”, “cuda”, “mps”) that should be used for computation.
    If None, checks if a GPU can be used.
    Note that macOS ARM64 users can enable the GPU on their local machine
    by setting `device="mps"`.

  **batch_size**
  : The batch size to use during `transform`.

  **token_env_variable**
  : The name of the environment variable which stores your HuggingFace
    authentication token to download private models.
    Note that we only store the name of the variable but not the token itself.

  **cache_folder**
  : Path to store models. By default `~/skrub_data`.
    See [`skrub.datasets.get_data_dir()`](skrub.datasets.get_data_dirhtml.md#skrub.datasets.get_data_dir).
    Note that when unpickling `TextEncoder` on another machine,
    the `cache_folder` path needs to be accessible to store the downloaded model.

  **store_weights_in_pickle**
  : Whether or not to keep the loaded sentence-transformers model
    in the `TextEncoder` when pickling.
    - When set to False, the `_estimator` property is removed from
      the object to pickle, which significantly reduces the size of
      the serialized object. Note that when the serialized object is
      unpickled on another machine, the `TextEncoder` will try to download
      the sentence-transformer model again from HuggingFace Hub.
      This process could fail if, for example, the machine doesn’t have
      internet access. Additionally, if you use weights stored on disk
      that are *not* on the HuggingFace Hub (by passing a path to
      `model_name`), these weights will not be pickled either.
      Therefore you would need to copy them to the machine where you
      unpickle the `TextEncoder`.
    - When set to True, the `_estimator` property is included in
      the serialized object. Users deploying fine-tuned models stored on
      disk are recommended to use this option. Note that the machine
      where the `TextEncoder` is unpickled must have the same device than
      the machine where it was pickled.

  **random_state**
  : Used when the PCA dimension reduction mechanism is used, for reproducible
    results across multiple function calls.

  **verbose**
  : Verbose level, controls whether to show a progress bar or not during
    `transform`.
* **Attributes:**
  **input_name_**
  : The name of the fitted column, or “text_enc” if the column has no name.

  **pca_**
  : A fitted PCA to reduce the embedding dimensionality (either PCA or truncation,
    see the `n_components` parameter).

  **n_components_**
  : The number of dimensions of the embeddings after dimensionality
    reduction.

#### SEE ALSO
[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
: Encode string columns as a numeric array with the minhash method.

[`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder)
: Encode string columns by constructing latent topics.

[`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder)
: Fast n-gram encoding of string columns.

[`SimilarityEncoder`](skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder)
: Encode string columns as a numeric array with n-gram string similarity.

### Notes

This class uses a pre-trained model, so calling `fit` or `fit_transform`
will not train or fine-tune the model. Instead, the model is loaded from disk,
and a PCA is fitted to reduce the dimension of the language model’s output,
if `n_components` is not None.

When PCA is disabled, this class is essentially stateless, with loading the
pre-trained model from disk being the only difference between `fit_transform`
and `transform`.

Be aware that parallelizing this class (e.g., using
[`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) with `n_jobs` > 1) may be computationally
expensive. This is because a copy of the pre-trained model is loaded into memory
for each thread. Therefore, we recommend you to let the default n_jobs=None
(or set to 1) of the TableVectorizer and let pytorch handle parallelism.

If memory usage is a concern, check the characteristics of your selected model.

### References

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import TextEncoder
```

Let’s encode video comments using only 2 embedding dimensions:

```pycon
>>> enc = TextEncoder(
...    model_name='intfloat/e5-small-v2', n_components=2
... )
>>> X = pd.Series([
...   "The professor snatched a good interview out of the jaws of these questions.",
...   "Bookmarking this to watch later.",
...   "When you don't know the lyrics of the song except the chorus",
... ], name='video comments')
```

Fitting does not train the underlying pre-trained deep-learning model,
but ensure various checks and enable dimension reduction.

```pycon
>>> enc.fit_transform(X)
   video comments_0  video comments_1
0          0.411395          0.096504
1         -0.105210         -0.344567
2         -0.306184          0.248063
```

### Methods

| [`fit`](#skrub.TextEncoder.fit)(column[, y])                                          | Fit the transformer.                                                                   |
|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| [`fit_transform`](#skrub.TextEncoder.fit_transform)(column[, y])                      | Fit the TextEncoder from `column`.                                                     |
| [`get_feature_names_out`](#skrub.TextEncoder.get_feature_names_out)([input_features]) | Return a list of features generated by the transformer.                                |
| [`get_params`](#skrub.TextEncoder.get_params)([deep])                                 | Get parameters for this estimator.                                                     |
| [`set_output`](#skrub.TextEncoder.set_output)(\*[, transform])                        | Default no-op implementation for set_output.                                           |
| [`set_params`](#skrub.TextEncoder.set_params)(\*\*params)                             | Set the parameters of this estimator.                                                  |
| [`set_transform_request`](#skrub.TextEncoder.set_transform_request)(\*[, column])     | Configure whether metadata should be requested to be passed to the `transform` method. |
| [`transform`](#skrub.TextEncoder.transform)(column)                                   | Transform `column` using the TextEncoder.                                              |
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

#### fit_transform(column, y=None)

Fit the TextEncoder from `column`.

In practice, it loads the pre-trained model from disk and returns
the embeddings of the column.

* **Parameters:**
  **column**
  : The string column to compute embeddings from.

  **y**
  : Unused. Here for compatibility with scikit-learn.
* **Returns:**
  **X_out**
  : The embedding representation of the input.

<!-- !! processed by numpydoc !! -->

#### get_feature_names_out(input_features=None)

Return a list of features generated by the transformer.

Each feature has format `{input_name}_{n_component}` where `input_name`
is the name of the input column, or a default name for the encoder, and
`n_component` is the idx of the specific feature.

* **Parameters:**
  **input_features**
  : The input features. Ignored, only here for compatibility.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The list of feature names.

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

#### set_transform_request(, column='$UNCHANGED$')

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
  **column**
  : Metadata routing for `column` parameter in `transform`.
* **Returns:**
  **self**
  : The updated object.

<!-- !! processed by numpydoc !! -->

#### transform(column)

Transform `column` using the TextEncoder.

This method uses the embedding model loaded in memory during `fit`
or `fit_transform`.

* **Parameters:**
  **column**
  : The string column to compute embeddings from.
* **Returns:**
  **X_out**
  : The embedding representation of the input.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div>
<!-- thumbnail-parent-div-close --></div>
