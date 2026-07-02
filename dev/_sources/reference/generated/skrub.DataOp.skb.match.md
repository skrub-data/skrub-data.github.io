# skrub.DataOp.skb.match

#### DataOp.skb.match(targets, default=NULL)

Select based on the value of a DataOp.

Evaluate `self`, then compare the result to the keys in `targets`.
If there is a match, evaluate the corresponding value and return it. If
there is no match and `default` has been provided, evaluate and return
`default`. Otherwise, a `KeyError` is raised.

Therefore, only one of the branches or the default is evaluated which
is the main advantage compared to placing the selection inside a
`@skrub.deferred` function.

* **Parameters:**
  **targets**
  : A dictionary providing which result to select. The keys must be
    actual values, they **cannot** be DataOps. The values can be
    DataOps or any object.

  **default**
  : If provided, the match falls back to the default when none of the
    targets have matched.
* **Returns:**
  The value corresponding to the matching key or the default

#### SEE ALSO
[`skrub.deferred`](skrub.deferredhtml.md#skrub.deferred)
: Wrap function calls in a [`DataOp`](skrub.DataOphtml.md#skrub.DataOp).

### Examples

```pycon
>>> import skrub
>>> a = skrub.var("a")
>>> mode = skrub.var("mode")
```

```pycon
>>> @skrub.deferred
... def mul(value, factor):
...     result = value * factor
...     print(f"{value} * {factor} = {result}")
...     return result
```

```pycon
>>> b = mode.skb.match(
...     {"one": mul(a, 1.0), "two": mul(a, 2.0), "three": mul(a, 3.0)},
...     default=mul(a, -1.0),
... )
>>> b.skb.eval({"a": 10.0, "mode": "two"})
10.0 * 2.0 = 20.0
20.0
>>> b.skb.eval({"a": 10.0, "mode": "three"})
10.0 * 3.0 = 30.0
30.0
>>> b.skb.eval({"a": 10.0, "mode": "twenty"})
10.0 * -1.0 = -10.0
-10.0
```

Note that only one of the multiplications gets evaluated.

<!-- !! processed by numpydoc !! -->
