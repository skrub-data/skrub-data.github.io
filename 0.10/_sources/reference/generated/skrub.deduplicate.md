# deduplicate

### skrub.deduplicate(X, , n_clusters=None, ngram_range=(2, 4), analyzer='char_wb', linkage_method='average', n_jobs=None)

Deduplicate categorical data by hierarchically clustering similar strings.

This works best if there are a number of underlying categories that
sometimes appear in the data with small variations and/or misspellings.

#### WARNING
This method can be computationally expensive for large datasets, as it
requires computing pairwise distances between unique values and performing
hierarchical clustering.

Additionally, this is an unsupervised method that
relies on the assumption that the most common spelling in each cluster is
the correct one.
Depending on the use case, this assumption may not always hold true.

* **Parameters:**
  **X**
  : The data to be deduplicated.

  **n_clusters**
  : Number of clusters to use for hierarchical clustering, if `None` use the
    number of clusters that lead to the lowest silhouette score.

  **ngram_range**
  : The lower and upper boundaries of the range of n-values for different
    n-grams used in the string similarity. All values of `n` such
    that `min_n <= n <= max_n` will be used.

  **analyzer**
  : Analyzer parameter for the CountVectorizer
    used for the string similarities.
    Describes whether the matrix `V` to factorize should be made of
    word counts or character n-gram counts.
    Option `char_wb` creates character n-grams only from text inside word
    boundaries; n-grams at the edges of words are padded with space.

  **linkage_method**
  : default=’average’
    Linkage method parameter to use for merging clusters via
    [`scipy.cluster.hierarchy.linkage()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.cluster.hierarchy.linkage.html#scipy.cluster.hierarchy.linkage).
    Option ‘average’ calculates the distance between two clusters as the
    average distance between data points in the first and second cluster.

  **n_jobs**
  : The number of jobs to run in parallel.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : The deduplicated data.

#### SEE ALSO
[`GapEncoder`](skrub.GapEncoderhtml.md#skrub.GapEncoder)
: Encodes dirty categories (strings) by constructing latent topics with continuous encoding.

[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)
: Encode string columns as a numeric array with the minhash method.

[`SimilarityEncoder`](skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder)
: Encode string columns as a numeric array with n-gram string similarity.

### Notes

Deduplication is done by first computing the n-gram distance between unique
categories in data, then performing hierarchical clustering on this distance
matrix, and choosing the most frequent element in each cluster as the
‘correct’ spelling. This method works best if the true number of
categories is significantly smaller than the number of observed spellings.

### Examples

```pycon
>>> from skrub.datasets import make_deduplication_data
>>> duplicated = make_deduplication_data(examples=['black', 'white'],
...                                      entries_per_example=[5, 5],
...                                      prob_mistake_per_letter=0.3,
...                                      random_state=42)
>>> duplicated
['blacs', 'black', 'black', 'black', 'black', 'uhibe', 'white', 'white', 'white', 'white']
```

To deduplicate the data, we can build a correspondence matrix:

```pycon
>>> from skrub import deduplicate
>>> deduplicate_correspondence = deduplicate(duplicated)
>>> deduplicate_correspondence
blacs    black
black    black
black    black
black    black
black    black
uhibe    white
white    white
white    white
white    white
white    white
dtype: ...
```

The translation table above is actually a series, giving the deduplicated values,
and indexed by the original values.
A deduplicated version of the initial list can easily be created:

```pycon
>>> deduplicated = list(deduplicate_correspondence)
>>> deduplicated
['black', 'black', 'black', 'black', 'black', 'white', 'white', 'white', 'white', 'white']
```

It is possible to use the deduplication function to replace values in a DataFrame
column:

```pycon
>>> import pandas as pd
>>> df = pd.DataFrame({'color': duplicated, 'value': range(10)})
>>> df
color  value
0  blacs      0
1  black      1
2  black      2
3  black      3
4  black      4
5  uhibe      5
6  white      6
7  white      7
8  white      8
9  white      9
>>> df['deduplicated_color'] = df['color'].map(deduplicate_correspondence.to_dict())
>>> df
color  value deduplicated_color
0  blacs      0              black
1  black      1              black
2  black      2              black
3  black      3              black
4  black      4              black
5  uhibe      5              white
6  white      6              white
7  white      7              white
8  white      8              white
9  white      9              white
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Real-world datasets often come with misspellings, for instance in manually inputted categorical variables. Such misspellings break data analysis steps that require exact matching, such as a GROUP BY operation.">  <div class="sphx-glr-thumbnail-title">Deduplicating misspelled categories</div>
</div>
<!-- thumbnail-parent-div-close --></div>
