# StringEncoder

### *class* skrub.StringEncoder(n_components=30, vectorizer='tfidf', ngram_range=(3, 4), analyzer='char_wb', stop_words=None, random_state=None, vocabulary=None)

Encode string features by using tf-idf vectorization and truncated singular     value decomposition (SVD).

First, apply a tf-idf vectorization of the text, then reduce the dimensionality
with a truncated SVD with the given number of parameters.

New features will be named `{col_name}_{component}` if the series has a name,
and `tsvd_{component}` if it does not.

* **Parameters:**
  **n_components**
  : Number of components to be used for the singular value decomposition (SVD).
    Must be a positive integer.

  **vectorizer**
  : Vectorizer to apply to the strings, either `tfidf` or `hashing` for
    scikit-learn TfidfVectorizer or HashingVectorizer respectively.

  **ngram_range**
  : The lower and upper boundary of the range of n-values for different
    n-grams to be extracted. All values of n such that min_n <= n <= max_n
    will be used. For example an `ngram_range` of `(1, 1)` means only unigrams,
    `(1, 2)` means unigrams and bigrams, and `(2, 2)` means only bigrams.

  **analyzer**
  : Whether the feature should be made of word or character n-grams.
    Option `char_wb` creates character n-grams only from text inside word
    boundaries; n-grams at the edges of words are padded with space.

  **stop_words**
  : If ‘english’, a built-in stop word list for English is used. There are several
    known issues with ‘english’ and you should consider an alternative (see [Using
    stop words](https://scikit-learn.org/stable/modules/feature_extraction.html#using-stop-words)).
    <br/>
    If a list, that list is assumed to contain stop words, all of which will be
    removed from the resulting tokens. Only applies if `analyzer == 'word'`.
    <br/>
    If None, no stop words will be used.

  **random_state**
  : Used during randomized svd. Pass an int for reproducible results across
    multiple function calls.

  **vocabulary**
  : In case of “tfidf” vectorizer, the vocabulary mapping passed to the vectorizer.
    Either a Mapping (e.g., a dict) where keys are terms and values are
    indices in the feature matrix, or an iterable over terms.
* **Attributes:**
  **input_name_**
  : Name of the fitted column, or “string_enc” if the column has no name.

  **n_components_**
  : The number of dimensions of the embeddings after dimensionality
    reduction.

  **all_outputs_**
  : A list that contains the name of all the features generated from the fitted
    column.

#### SEE ALSO
[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
: Encode string columns as a numeric array with the minhash method.

[`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder)
: Encode string columns by constructing latent topics.

[`TextEncoder`](skrub.TextEncoderhtml.md#skrub.TextEncoder)
: Encode string columns using pre-trained language models.

### Notes

Skrub provides `StringEncoder` as a simple interface to perform [Latent Semantic
Analysis (LSA)](https://scikit-learn.org/stable/modules/decomposition.html#about-truncated-svd-and-latent-semantic-analysis-(lsa)).
As such, it doesn’t support all hyper-parameters exposed by the underlying
{[`TfidfVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer),
[`HashingVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.HashingVectorizer.html#sklearn.feature_extraction.text.HashingVectorizer)} and
[`TruncatedSVD`](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html#sklearn.decomposition.TruncatedSVD). If you need more flexibility than the
proposed hyper-parameters of `StringEncoder`, you must create your own LSA using
scikit-learn [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline), such as:

```pycon
>>> from sklearn.pipeline import make_pipeline
>>> from sklearn.feature_extraction.text import TfidfVectorizer
>>> from sklearn.decomposition import TruncatedSVD
```

```pycon
>>> make_pipeline(TfidfVectorizer(max_df=300), TruncatedSVD())
Pipeline(steps=[('tfidfvectorizer', TfidfVectorizer(max_df=300)),
            ('truncatedsvd', TruncatedSVD())])
```

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import StringEncoder
```

We will encode the comments using 2 components:

```pycon
>>> enc = StringEncoder(n_components=2)
>>> X = pd.Series([
...   "The professor snatched a good interview out of the jaws of these questions.",
...   "Bookmarking this to watch later.",
...   "When you don't know the lyrics of the song except the chorus",
... ], name='video comments')
```

```pycon
>>> enc.fit_transform(X)
   video comments_0  video comments_1
0          1.322973         -0.163070
1          0.379688          1.659319
2          1.306400         -0.317120
```

### Methods

| [`fit`](#skrub.StringEncoder.fit)(column[, y])                                          | Fit the transformer.                                    |
|-----------------------------------------------------------------------------------------|---------------------------------------------------------|
| [`fit_transform`](#skrub.StringEncoder.fit_transform)(X[, y])                           | Fit the encoder and transform a column.                 |
| [`get_feature_names_out`](#skrub.StringEncoder.get_feature_names_out)([input_features]) | Return a list of features generated by the transformer. |
| [`get_params`](#skrub.StringEncoder.get_params)([deep])                                 | Get parameters for this estimator.                      |
| [`set_output`](#skrub.StringEncoder.set_output)(\*[, transform])                        | Set output container.                                   |
| [`set_params`](#skrub.StringEncoder.set_params)(\*\*params)                             | Set the parameters of this estimator.                   |
| [`transform`](#skrub.StringEncoder.transform)(X)                                        | Transform a column.                                     |
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

#### fit_transform(X, y=None)

Fit the encoder and transform a column.

* **Parameters:**
  **X**
  : The column to transform.

  **y**
  : Unused. Here for compatibility with scikit-learn.
* **Returns:**
  X_out: Pandas or Polars dataframe with shape (len(X), tsvd_n_components)
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

#### transform(X)

Transform a column.

* **Parameters:**
  **X**
  : The column to transform.
* **Returns:**
  result: Pandas or Polars dataframe with shape (len(X), tsvd_n_components)
  : The embedding representation of the input.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
