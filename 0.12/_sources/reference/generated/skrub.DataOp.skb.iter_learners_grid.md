# skrub.DataOp.skb.iter_learners_grid

#### DataOp.skb.iter_learners_grid()

Get learners with different parameter combinations.

This generator yields a [`SkrubLearner`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner) parametrized for each
possible combination of choices.

The choice outcomes used in each learner can be inspected with
[`SkrubLearner.describe_params()`](skrub.SkrubLearnerhtml.md#skrub.SkrubLearner.describe_params).

#### SEE ALSO
[`DataOp.skb.iter_learners_randomized`](skrub.DataOp.skb.iter_learners_randomizedhtml.md#skrub.DataOp.skb.iter_learners_randomized)
: Similar function but for random sampling of the parameter space. Must be used when the DataOp contains some numeric ranges built with [`choose_float()`](skrub.choose_floathtml.md#skrub.choose_float) or [`choose_int()`](skrub.choose_inthtml.md#skrub.choose_int) with `n_steps=None`.

[`DataOp.skb.make_grid_search`](skrub.DataOp.skb.make_grid_searchhtml.md#skrub.DataOp.skb.make_grid_search)
: Learner with built-in exhaustive exploration of the parameter grid to select the best one.

[`DataOp.skb.make_randomized_search`](skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search)
: Learner with built-in randomized exploration of the parameter grid to select the best one.

### Examples

```pycon
>>> import numpy as np
>>> from sklearn import preprocessing
>>> import skrub
```

```pycon
>>> scaler = skrub.choose_from(
...     [
...         preprocessing.MinMaxScaler(),
...         preprocessing.StandardScaler(),
...         preprocessing.RobustScaler(),
...         preprocessing.MaxAbsScaler(),
...     ],
...     name="scaler",
... )
>>> out = skrub.X().skb.apply(scaler)
```

```pycon
>>> X = np.asarray([-4.0, 3.0, 10.0])[:, None]
```

```pycon
>>> for p in out.skb.iter_learners_grid():
...     print("======================================")
...     print("params:", p.describe_params())
...     print("result:")
...     print(p.fit_transform({"X": X}))
======================================
params: {'scaler': 'MinMaxScaler()'}
result:
[[0. ]
 [0.5]
 [1. ]]
======================================
params: {'scaler': 'StandardScaler()'}
result:
[[-1.22474487]
 [ 0.        ]
 [ 1.22474487]]
======================================
params: {'scaler': 'RobustScaler()'}
result:
[[-1.]
 [ 0.]
 [ 1.]]
======================================
params: {'scaler': 'MaxAbsScaler()'}
result:
[[-0.4]
 [ 0.3]
 [ 1. ]]
```

<!-- !! processed by numpydoc !! -->
