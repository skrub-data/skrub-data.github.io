# skrub.DataOp.skb.make_learner

#### DataOp.skb.make_learner(, fitted=False, keep_subsampling=False, choose='default')

Get a skrub learner for this DataOp.

Returns a [`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) with a `fit()` method so it can be fit
to some training data and then apply it to unseen data by calling
`transform()` or `predict()`. Unlike scikit-learn estimators, skrub
learners accept a dictionary of inputs rather than `X` and `y` arguments.

#### WARNING
If the DataOp contains choices (e.g. `choose_from(...)`), by
default this learner uses the default value of each choice. See the
`choose` parameter for other options (random or from an
[Optuna](https://optuna.readthedocs.io/en/stable/) trial). To actually
pick the best value with hyperparameter tuning, use
[`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
[`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search) instead, or an Optuna
[`Study`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.Study.html#optuna.study.Study) as shown in this
[example](../../auto_examples/02_data_ops/1131_optuna_choiceshtml.md#example-optuna-choices).

* **Parameters:**
  **fitted**
  : If true, the returned learner is fitted to the data provided when
    initializing variables in `skrub.var("name", value=...)` and
    `skrub.X(value)`.

  **keep_subsampling**
  : If True, and if subsampling has been configured (see
    [`DataOp.skb.subsample()`](skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample)), fit on a subsample of the data. By
    default subsampling is not applied and all the data is used. This
    is only applied for fitting the estimator when `fitted=True`,
    subsequent use of the estimator is not affected by subsampling.
    Therefore it is an error to pass `keep_subsampling=True` and
    `fitted=False` (because `keep_subsampling=True` would have no
    effect).

  **choose**
  : How to resolve choices contained in the data_op. The different
    options are:
    - ‘default’: the corresponding parameters of the SkrubLearner are not
      set; the default values of the choices are used.
    - ‘random’: a random value is picked according to the distribution of
      each choice. The form ‘random([seed])’ is also accepted to set
      the random seed: for example ‘random(0)’ sets it to 0. ‘random()’
      is the same as ‘random’.
    - an instance of [`numpy.random.RandomState`](https://numpy.org/doc/stable/reference/random/legacy.html#numpy.random.RandomState). Same as ‘random’,
      but the provided RandomState is used to sample values.
    - an instance of [`optuna.Trial`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.trial.Trial.html#optuna.trial.Trial) or
      [`optuna.FrozenTrial`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.trial.FrozenTrial.html#optuna.trial.FrozenTrial). It is
      used to suggest values for
      the choices.
    <br/>
    Note that none of these options picks the best choice value according to
    an evaluation criterion, as this function creates a single learner.
    These options can be combined with external logic to evaluate and
    select the resulting learners, or one of
    [`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search),
    [`DataOp.skb.make_randomized_search()`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search),
    [`optuna.Study.optimize`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.study.Study.html#optuna.study.Study.optimize) (as
    shown in this [example](../../auto_examples/02_data_ops/1131_optuna_choiceshtml.md#example-optuna-choices)) can be used
    to automatically select the best hyperparameters.
* **Returns:**
  learner
  : A skrub learner with an interface similar to scikit-learn’s, except
    that its methods accept a dictionary of named inputs rather than
    `X` and `y` arguments.

### Examples

```pycon
>>> import skrub
>>> from sklearn.dummy import DummyClassifier
>>> orders_df = skrub.datasets.toy_orders(split="train").orders
>>> orders = skrub.var('orders', orders_df)
>>> X = orders.drop(columns='delayed', errors='ignore').skb.mark_as_X()
>>> y = orders['delayed'].skb.mark_as_y()
>>> pred = X.skb.apply(skrub.TableVectorizer()).skb.apply(
...     DummyClassifier(), y=y
... )
>>> pred
<Apply DummyClassifier>
Result:
―――――――
0    False
1    False
2    False
3    False
Name: delayed, dtype: bool
>>> learner = pred.skb.make_learner(fitted=True)
>>> new_orders_df = skrub.datasets.toy_orders(split='test').X
>>> new_orders_df
   ID product  quantity        date
4   5     cup         5  2020-04-11
5   6    fork         2  2020-04-12
>>> learner.predict({'orders': new_orders_df})
array([False, False])
```

Note that the `'orders'` key in the dictionary passed to `predict`
corresponds to the name `'orders'` in `skrub.var('orders',
orders_df)` above.

The `choose` parameter allows us to control how choices contained in
the DataOp should be handled. The default is to use the default value
of each choice.

```pycon
>>> def mult(x, factor):
...     return x * factor
>>> out = skrub.var("x").skb.apply_func(
...     mult, skrub.choose_int(-10, 10, default=2)
... )
>>> out.skb.make_learner().fit_transform({'x': 1})
2
```

The ‘random’ option samples new choice outcomes for each created learner:

```pycon
>>> out.skb.make_learner(choose='random').fit_transform({'x': 1})
np.int64(3)
>>> out.skb.make_learner(choose='random').fit_transform({'x': 1})
np.int64(-5)
```

If an [`optuna.Trial`](https://optuna.readthedocs.io/en/stable/reference/generated/optuna.trial.Trial.html#optuna.trial.Trial) instance is passed
instead, the choice outcomes are obtained by calling the trial’s
`suggest_int`, `suggest_float` or `suggest_categorical` methods.
This allows easily selecting hyperparameters with optuna.

Please see the examples gallery for full information about DataOps
and the learners they generate.

<!-- !! processed by numpydoc !! -->
