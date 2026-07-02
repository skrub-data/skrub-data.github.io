# optional

### skrub.optional(value, , name=None, default=value)

A choice between `value` and `None`.

Typically, `value` is an estimator and this choice is passed to
[`DataOp.skb.apply()`](skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply).

`optional` allows to build a branch in the search space where the `value`
is a component of the pipeline
(e.g., a dimensionality reduction step, a feature selection step, etc) which may
be present or not. When the component is not present, it is represented by `None`.
This is equivalent to `skrub.choose_from([value, None], name=name)`.

When a learner is fitted *without hyperparameter tuning*, the outcome of
this choice is `value`. Pass `default=None` to make `None` the
default outcome.

* **Parameters:**
  **value**
  : The outcome (when `None` is not chosen).

  **name**
  : If not `None`, `name` is used when displaying search results and
    can also be used to override the choice’s value by setting it in the
    environment containing a learner’s inputs.

  **default**
  : An `optional` is a choice between the provided `value` and
    `None`. Normally, the default outcome when a learner is used
    *without hyperparameter tuning* is the provided `value`. Pass
    `default=None` to make the alternative outcome, `None`, the
    default. `None` is the only allowed value for this parameter.
* **Returns:**
  Choice
  : An object representing this choice, which can be used in a skrub
    learner.

### Examples

`optional` is useful for optional steps in a DataOps plan, or a learner.
If we want to try a learner with or without dimensionality reduction, we can
add a step such as:

```pycon
>>> from sklearn.decomposition import PCA
>>> from skrub import optional
>>> optional(PCA(), name='use dim reduction')
optional(PCA(), name='use dim reduction')
```

The constructed parameter grid will include a branch of the plan with the
the PCA and one without:

```pycon
>>> print(
... optional(PCA(), name='dim reduction').as_data_op().skb.describe_param_grid()
... )
- dim reduction: [PCA(), None]
```

When a learner that contains an `optional` step is used *without
hyperparameter tuning*, the default outcome is the provided `value`.

```pycon
>>> print(optional(PCA()).default())
PCA()
```

This can be overridden by passing `default=None`:

```pycon
>>> print(optional(PCA(), default=None).default())
None
```

In practice, `optional` is used with [`DataOp.skb.apply()`](skrub.DataOp.skb.applyhtml.md#skrub.DataOp.skb.apply) to make the
application of a transformer optional.
For example, if we want to make the application of PCA optional, we can do:

```pycon
>>> import skrub
>>> from skrub.datasets import toy_products
>>> from sklearn.decomposition import PCA
```

```pycon
>>> products = skrub.var("products", toy_products())
>>> vectorized = products.skb.apply(skrub.TableVectorizer())
>>> reduced = vectorized.skb.apply(skrub.optional(PCA(n_components=2), name="pca"))
>>> print(reduced.skb.describe_param_grid())
- pca: [PCA(n_components=2), None]
```

If we perform a hyperparameter search (for example with
[`DataOp.skb.make_grid_search()`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)), both pipelines (with and without a PCA)
will be considered and the one giving the best predictions will be selected.
See also
——–
choose_bool :

> Construct a choice between False and True.

choose_float :
: Construct a choice of floating-point numbers from a numeric range.

choose_from :
: Construct a choice among several possible outcomes.

choose_int :
: Construct a choice of integers from a numeric range.

<!-- !! processed by numpydoc !! -->
