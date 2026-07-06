# skrub.DataOp.skb.if_else

#### DataOp.skb.if_else(value_if_true, value_if_false)

Create a conditional DataOp.

If `self` evaluates to `True`, the result will be
`value_if_true`, otherwise `value_if_false`.

* **Parameters:**
  **value_if_true**
  : The value to return if `self` is true.

  **value_if_false**
  : The value to return if `self` is false.
* **Returns:**
  Conditional DataOp

#### SEE ALSO
[`skrub.DataOp.skb.match`](skrub.DataOp.skb.matchhtml.md#skrub.DataOp.skb.match)
: Select based on the value of a DataOp.

### Notes

The branch which is not selected is not evaluated, which is the main
advantage compared to wrapping the conditional statement in a
`@skrub.deferred` function.

### Examples

```pycon
>>> import skrub
>>> import numpy as np
>>> a = skrub.var('a')
>>> shuffle_a = skrub.var('shuffle_a')
```

```pycon
>>> @skrub.deferred
... def shuffled(values):
...     print('shuffling')
...     return np.random.default_rng(0).permutation(values)
```

```pycon
>>> @skrub.deferred
... def copy(values):
...     print('copying')
...     return values.copy()
```

```pycon
>>> b = shuffle_a.skb.if_else(shuffled(a), copy(a))
>>> b
<IfElse <Var 'shuffle_a'> ? <Call 'shuffled'> : <Call 'copy'>>
```

Note that only one of the 2 branches is evaluated:

```pycon
>>> b.skb.eval({'a': np.arange(3), 'shuffle_a': True})
shuffling
array([2, 0, 1])
>>> b.skb.eval({'a': np.arange(3), 'shuffle_a': False})
copying
array([0, 1, 2])
```

<!-- !! processed by numpydoc !! -->
