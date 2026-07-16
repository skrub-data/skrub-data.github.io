# GapEncoder

### *class* skrub.GapEncoder(n_components=10, batch_size=1024, gamma_shape_prior=1.1, gamma_scale_prior=1.0, rho=0.95, rescale_rho=False, hashing=False, hashing_n_features=4096, init='k-means++', max_iter=5, ngram_range=(2, 4), analyzer='char', add_words=False, random_state=None, rescale_W=True, max_iter_e_step=1, max_no_improvement=5, verbose=0)

Encode string columns by constructing latent topics.

This encoder can be understood as a continuous encoding on a set of latent
categories estimated from the data. The latent categories are built by
capturing combinations of substrings that frequently co-occur.

The GapEncoder supports online learning on batches of
data for scalability through the GapEncoder.partial_fit
method.

The principle is as follows:

1. Given an input string array `X`, we build its bag-of-n-grams
   representation `V` (`n_samples`, `vocab_size`).
2. Instead of using the n-grams counts as encodings, we look for low-
   dimensional representations by modeling n-grams counts as linear
   combinations of topics `V = HW`, with `W` (`n_topics`, `vocab_size`)
   the topics and `H` (`n_samples`, `n_topics`) the associated activations.
3. Assuming that n-grams counts follow a Poisson law, we fit `H` and `W` to
   maximize the likelihood of the data, with a Gamma prior for the
   activations `H` to induce sparsity.
4. In practice, this is equivalent to a non-negative matrix factorization
   with the Kullback-Leibler divergence as loss, and a Gamma prior on `H`.
   We thus optimize `H` and `W` with the multiplicative update method.

“Gap” stands for “Gamma-Poisson”, the families of distributions that are
used to model the importance of topics in a document (Gamma), and the term
frequencies in a document (Poisson).

Input columns that do not have a string or Categorical dtype are rejected
by raising a `RejectColumn` exception.

* **Parameters:**
  **n_components**
  : Number of latent categories used to model string data.

  **batch_size**
  : Number of samples per batch.

  **gamma_shape_prior**
  : Shape parameter for the Gamma prior distribution.

  **gamma_scale_prior**
  : Scale parameter for the Gamma prior distribution.

  **rho**
  : Weight parameter for the update of the `W` matrix.

  **rescale_rho**
  : If `True`, use `rho ** (batch_size / len(X))` instead of rho to obtain
    an update rate per iteration that is independent of the batch size.

  **hashing**
  : If `True`, HashingVectorizer is used instead of CountVectorizer.
    It has the advantage of being very low memory, scalable to large
    datasets as there is no need to store a vocabulary dictionary in
    memory.

  **hashing_n_features**
  : Number of features for the HashingVectorizer.
    Only relevant if `hashing=True`.

  **init**
  : Initialization method of the `W` matrix.
    If `init='k-means++'`, we use the init method of KMeans.
    If `init='random'`, topics are initialized with a Gamma distribution.
    If `init='k-means'`, topics are initialized with a KMeans on the
    n-grams counts.

  **max_iter**
  : Maximum number of iterations on the input data.

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

  **add_words**
  : If `True`, add the words counts to the bag-of-n-grams representation
    of the input data.

  **random_state**
  : Random number generator seed for reproducible output across multiple
    function calls.

  **rescale_W**
  : If `True`, the weight matrix `W` is rescaled at each iteration
    to have a l1 norm equal to 1 for each row.

  **max_iter_e_step**
  : Maximum number of iterations to adjust the activations h at each step.

  **max_no_improvement**
  : Control early stopping based on the consecutive number of mini batches
    that do not yield an improvement on the smoothed cost function.
    To disable early stopping and run the process fully,
    set `max_no_improvement=None`.

  **verbose**
  : Verbosity level. The higher, the more granular the logging.
* **Attributes:**
  **rho_**
  : Effective update rate for the `W` matrix.

  **fitted_models_**
  : Column-wise fitted GapEncoders.

  **column_names_**
  : Column names of the data the Gap was fitted on.

#### SEE ALSO
[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
: Encode string columns as a numeric array with the minhash method.

[`SimilarityEncoder`](skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder)
: Encode string columns as a numeric array with n-gram string similarity.

[`TextEncoder`](skrub.TextEncoderhtml.md#skrub.TextEncoder)
: Encode string columns with a pretrained language model.

[`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder)
: Fast n-gram encoding of string columns.

[`deduplicate`](skrub.deduplicatehtml.md#skrub.deduplicate)
: Deduplicate data by hierarchically clustering similar strings.

### References

For a detailed description of the method, see
[Encoding high-cardinality string categorical variables](https://hal.inria.fr/hal-02171256v4) by Cerda, Varoquaux (2019).

### Examples

```pycon
>>> import pandas as pd
>>> from skrub import GapEncoder
>>> enc = GapEncoder(n_components=2, random_state=0)
```

Let’s encode the following non-normalized data:

```pycon
>>> X = pd.Series(['Paris, FR', 'Paris', 'London, UK', 'Paris, France',
...                'london', 'London, England', 'London', 'Pqris'], name='city')
>>> enc.fit(X)
GapEncoder(n_components=2, random_state=0)
```

The GapEncoder has found the following two topics:

```pycon
>>> enc.get_feature_names_out()
['city: england, london, uk', 'city: france, paris, pqris']
```

It got it right, reoccurring topics are “London” and “England” on the
one side and “Paris” and “France” on the other.

As this is a continuous encoding, we can look at the level of
activation of each topic for each category:

```pycon
>>> enc.transform(X)
   city: england, london, uk  city: france, paris, pqris
0                   0.051816                   10.548184
1                   0.050134                    4.549866
2                  12.046517                    0.053483
3                   0.052270                   16.547730
4                   6.049970                    0.050030
5                  19.545227                    0.054773
6                   6.049970                    0.050030
7                   0.060120                    4.539880
```

The higher the value, the bigger the correspondence with the topic.

### Methods

| [`fit`](#skrub.GapEncoder.fit)(X[, y])                                                         | Fit the GapEncoder and transform a column.                            |
|------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| [`fit_transform`](#skrub.GapEncoder.fit_transform)(X[, y])                                     | Fit to data, then transform it.                                       |
| [`get_feature_names_out`](#skrub.GapEncoder.get_feature_names_out)([input_features, n_labels]) | Return the labels that best summarize the learned components/topics.  |
| [`get_params`](#skrub.GapEncoder.get_params)([deep])                                           | Get parameters for this estimator.                                    |
| [`partial_fit`](#skrub.GapEncoder.partial_fit)(X[, y])                                         | Partial fit this instance on `X`.                                     |
| [`score`](#skrub.GapEncoder.score)(X)                                                          | Score this instance of `X`.                                           |
| [`set_output`](#skrub.GapEncoder.set_output)(\*[, transform])                                  | Set output container.                                                 |
| [`set_params`](#skrub.GapEncoder.set_params)(\*\*params)                                       | Set the parameters of this estimator.                                 |
| [`transform`](#skrub.GapEncoder.transform)(X)                                                  | Return the encoded vectors (activations) `H` of input strings in `X`. |
<!-- !! processed by numpydoc !! -->

#### fit(X, y=None)

Fit the GapEncoder and transform a column.

* **Parameters:**
  **X**
  : The string data to fit the model on.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`GapEncoder`](#skrub.GapEncoder)
  : The fitted GapEncoder instance (self).

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

#### get_feature_names_out(input_features=None, n_labels=3)

Return the labels that best summarize the learned components/topics.

For each topic, labels with the highest activations are selected.

* **Parameters:**
  **input_features**
  : The input features. Ignored, only here for compatibility.

  **n_labels**
  : The number of labels used to describe each topic.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The labels that best describe each topic.

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

#### partial_fit(X, y=None)

Partial fit this instance on `X`.

To be used in an online learning procedure where batches of data are
coming one by one.

* **Parameters:**
  **X**
  : The string data to fit the model on.

  **y**
  : Unused, only here for compatibility.
* **Returns:**
  [`GapEncoder`](#skrub.GapEncoder)
  : The fitted GapEncoder instance (self).

<!-- !! processed by numpydoc !! -->

#### score(X)

Score this instance of `X`.

Returns the Kullback-Leibler divergence between the n-grams counts
matrix `V` of `X`, and its non-negative factorization `HW`.

* **Parameters:**
  **X**
  : The data to encode.
* **Returns:**
  [`float`](https://docs.python.org/3/library/functions.html#float)
  : The Kullback-Leibler divergence.

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

Return the encoded vectors (activations) `H` of input strings in `X`.

* **Parameters:**
  **X**
  : The string data to encode.
* **Returns:**
  DataFrame, shape (n_samples, n_topics)
  : Transformed input.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Real-world datasets often come with misspellings, for instance in manually inputted categorical variables. Such misspellings break data analysis steps that require exact matching, such as a GROUP BY operation.">  <div class="sphx-glr-thumbnail-title">Deduplicating misspelled categories</div>
</div>
<!-- thumbnail-parent-div-close --></div>
