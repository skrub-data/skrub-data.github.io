# fetch_movielens

### skrub.datasets.fetch_movielens(data_home=None)

Fetch the movielens dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is a regression use-case, where the goal is to predict movie ratings.
More details are provided in the output’s `metadata['description']`.
Size on disk: 3.6MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    movies
    : Dataframe with movie titles and genres.
    <br/>
    ratings
    : Dataframe with ratings of movies.
    <br/>
    metadata
    : A dictionary containing the name source and description.
    <br/>
    movies_path
    : The path to the movies CSV file.
    <br/>
    ratings_path
    : The path to the ratings CSV file.

<!-- !! processed by numpydoc !! -->
