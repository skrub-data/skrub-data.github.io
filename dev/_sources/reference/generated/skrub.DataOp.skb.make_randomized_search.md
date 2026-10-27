# skrub.DataOp.skb.make_randomized_search

#### DataOp.skb.make_randomized_search(, fitted=False, keep_subsampling=False, backend='sklearn', n_iter=10, scoring=None, n_jobs=None, refit=True, cv=None, verbose=0, pre_dispatch='2\*n_jobs', random_state=None, error_score=nan, return_train_score=False, storage=None, study_name=None, sampler=None, timeout=None)

Find the best parameters with randomized search.

This function returns a [`ParamSearch`](skrub.ParamSearchhtml.md#skrub.ParamSearch), an object similar to
scikit-learn’s [`RandomizedSearchCV`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html#sklearn.model_selection.RandomizedSearchCV), where
the main difference is `fit()` and `predict()` accept a
dictionary of inputs rather than `X` and `y`. The best learner is stored
in the attribute `.best_learner_`.

* **Parameters:**
  **fitted**
  : If `True`, the randomized search is fitted on the data provided when
    initializing variables in this DataOp (the data returned by
    `.skb.get_data()`).

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), fit on a subsample of the data. By
    default subsampling is not applied and all the data is used. This
    is only applied for fitting the randomized search when `fitted=True`,
    subsequent use of the randomized search is not affected by subsampling.
    Therefore it is an error to pass `keep_subsampling=True` and
    `fitted=False` (because `keep_subsampling=True` would have no
    effect).

  **backend**
  : Which library to use for hyperparameter search. The default is
    ‘sklearn’, which uses
    the scikit-learn [`RandomizedSearchCV`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html#sklearn.model_selection.RandomizedSearchCV).
    If ‘optuna’, an Optuna [`Study`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.Study.html#optuna.study.Study) is used instead
    and it is possible to choose the sampler and storage.

  **n_iter**
  : Number of parameter combinations to try.

  **scoring**
  : Strategy to evaluate the model’s predictions.
    It can be:
    - None: use the predictor’s `score` method
    - a metric name such as ‘accuracy’
    - a list of such names
    - a callable (estimator, X_test, y_test) → score
    - a dict mapping metric name to callable
    - a callable returning a dict mapping metric name to value
    <br/>
    See the [scikit-learn documentation](https://scikit-learn.org/stable/modules/model_evaluation.html#the-scoring-parameter-defining-model-evaluation-rules)
    for details.

  **n_jobs**
  : Number of jobs to run in parallel.
    `None` means 1 unless in a `joblib.parallel_backend` context.
    `-1` means using all processors.

  **refit**
  : Whether to refit a learner to the whole dataset using the best
    parameters found.
    <br/>
    For multiple metric evaluation it should be the name of the metric
    to use to pick the best parameters. If `backend='optuna'` it is
    also the metric that drives the Optuna optimization.

  **cv**
  : Cross-validation splitting strategy. It can be:
    - None: 5-fold (stratified) cross-validation
    - integer: specify the number of folds
    - sklearn [CV splitter](https://scikit-learn.org/stable/modules/cross_validation.html#cross-validation-iterators)
    - iterable yielding (train, test) splits as arrays of indices.

  **verbose**
  : Verbosity, the higher the more verbose. It is recommended to leave
    it to 0 if using `backend='optuna'`.

  **pre_dispatch**
  : Number of jobs dispatched during parallel execution, when using the
    joblib parallelization.

  **random_state**
  : Pseudo random number generator state used for random sampling
    Pass an int for reproducible output across multiple function calls.
    <br/>
    #### NOTE
    the result will never be deterministic if using
    `backend='optuna'` and `n_jobs > 1` (as the sampled
    parameters depend on previous runs).

  **error_score**
  : Value to assign to the score if an error occurs in estimator fitting.
    If set to ‘raise’, the error is raised.

  **return_train_score**
  : Also compute scores on the training set, in which case they will be
    available in the `cv_results_` attribute in addition to the
    scores on the test set.

  **storage**
  : The URL for the database to use as the Optuna storage. In addition
    to the usual relational database URLs (e.g.
    `'sqlite:///<file_path>'` ), it can be
    `'journal:///<file_path>'` to use Optuna’s
    [`JournalStorage`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.storages.JournalStorage.html#optuna.storages.JournalStorage). See the [SQLAlchemy
    documentation](https://docs.sqlalchemy.org/en/20/core/engines.html#database-urls)
    for information on how to construct database URLs, which take the
    general form
    `dialect+driver://username:password@host:port/database`.

  **study_name**
  : The name to use for the created (or loaded) Optuna study. If the
    study already exists in the provided `storage`, the existing one
    is loaded. If None, a random name is generated.

  **sampler**
  : The sampler to use when the backend is ‘optuna’. If None, a
    [`TPESampler`](https://optuna.readthedocs.io/en/stable/reference/samplers/generated/optuna.samplers.TPESampler.html#optuna.samplers.TPESampler) is used (the same default as
    [`create_study()`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.create_study.html#optuna.study.create_study)).

  **timeout**
  : Timeout after which no new trials are created. Trials already
    started when reaching the timeout are still completed. If None,
    there is no timeout and all `n_iters` trials are completed.
    <br/>
    #### NOTE
    If this parameter is used, parallelization when `n_jobs > 1`
    is always done with Optuna’s built-in parallelization, which
    relies on multithreading. This means threads are used (rather
    than processes) regardless of the joblib backend.
* **Returns:**
  ParamSearch or OptunaParamSearch
  : An object implementing the hyperparameter search. Besides the usual
    `fit`, `predict`, attributes of interest are
    `results_`, `plot_results()`, and `best_learner_`.
    If `backend='optuna'` was used, the returned object is an
    [`OptunaParamSearch`](skrub.OptunaParamSearchhtml.md#skrub.OptunaParamSearch) which additionally has an attribute
    `study_` which is the Optuna [`Study`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.Study.html#optuna.study.Study) that
    performed the hyperparameter optimization.

#### SEE ALSO
[`skrub.DataOp.skb.make_grid_search`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Find the best parameters with grid search.

[`skrub.DataOp.skb.make_learner`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)
: Make a [`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) without actually searching for the best hyperparameters. The strategy to resolve choices can be use the default value, random, or taking suggestions from an Optuna [`optuna.trial.Trial`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.trial.Trial.html#optuna.trial.Trial). This allows using Optuna directly, rather than through the `make_randomized_search` interface, for more advanced use cases.

### Examples

```pycon
>>> import skrub
>>> from sklearn.datasets import make_classification
>>> from sklearn.linear_model import LogisticRegression
>>> from sklearn.feature_selection import SelectKBest
>>> from sklearn.ensemble import RandomForestClassifier
>>> from sklearn.dummy import DummyClassifier
```

```pycon
>>> X_a, y_a = make_classification(random_state=0)
>>> X, y = skrub.X(X_a), skrub.y(y_a)
>>> selector = SelectKBest(k=skrub.choose_int(4, 20, log=True, name='k'))
>>> logistic = LogisticRegression(C=skrub.choose_float(0.1, 10.0, log=True, name="C"))
>>> rf = RandomForestClassifier(
...     n_estimators=skrub.choose_int(3, 30, log=True, name="N 🌴"),
...     random_state=0,
... )
>>> classifier = skrub.choose_from(
...     {"logistic": logistic, "rf": rf, "dummy": DummyClassifier()}, name="classifier"
... )
>>> pred = X.skb.apply(selector, y=y).skb.apply(classifier, y=y)
>>> print(pred.skb.describe_param_grid())
- k: choose_int(4, 20, log=True, name='k')
  classifier: 'logistic'
  C: choose_float(0.1, 10.0, log=True, name='C')
- k: choose_int(4, 20, log=True, name='k')
  classifier: 'rf'
  N 🌴: choose_int(3, 30, log=True, name='N 🌴')
- k: choose_int(4, 20, log=True, name='k')
  classifier: 'dummy'
```

```pycon
>>> search = pred.skb.make_randomized_search(fitted=True, random_state=0)
>>> search.results_
    k         C  N 🌴 classifier  mean_test_score
0   4  4.626363  NaN   logistic             0.92
1  16       NaN  6.0         rf             0.90
2  11       NaN  7.0         rf             0.88
3   7  3.832217  NaN   logistic             0.87
4  10  4.881255  NaN   logistic             0.85
5  20  3.965675  NaN   logistic             0.80
6  14       NaN  3.0         rf             0.77
7   4       NaN  NaN      dummy             0.50
8  10       NaN  NaN      dummy             0.50
9   5       NaN  NaN      dummy             0.50
```

Please refer to the examples gallery for an in-depth explanation.

<!-- !! processed by numpydoc !! -->
