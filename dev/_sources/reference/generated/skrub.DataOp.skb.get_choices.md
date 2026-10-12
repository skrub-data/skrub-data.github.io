# skrub.DataOp.skb.get_choices

#### DataOp.skb.get_choices(named_only=False)

Get all the choices used in the DataOp.

* **Parameters:**
  **named_only**
  : If True, only choices that have their `name` attribute set are
    returned. Those are the only names for which a value can be passed
    in the environment passed to `eval()`.
    <br/>
    If False, all choices are returned. For choices that have a
    `name`, that `name` is their key in the returned dictionary.
    For other choices, the key is arbitrary but stable for a same
    DataOp and its clones, and is the same string that is used in the
    parallel coordinate plots produced by
    [`ParamSearch.plot_results()`](skrub.ParamSearch.md#skrub.ParamSearch.plot_results).
* **Returns:**
  [`dict`](https://docs.python.org/3/builtins/stdtypes.html#dict)
  : Keys are names, and values the corresponding choice object.

#### SEE ALSO
[`DataOp.skb.get_vars`](skrub.DataOp.skb.get_vars.md#skrub.DataOp.skb.get_vars)
: Similar function for getting variables created with [`var()`](skrub.var.md#skrub.var) and DataOps whose name was set with [`.skb.set_name`](skrub.DataOp.skb.set_name.md#skrub.DataOp.skb.set_name).

[`DataOp.skb.describe_defaults`](skrub.DataOp.skb.describe_defaults.md#skrub.DataOp.skb.describe_defaults)
: Get a dictionary showing the default values for choices. Only choices that actually get used in the default configuration are shown.

[`SkrubLearner.get_named_params`](skrub.SkrubLearner.md#skrub.SkrubLearner.get_named_params)
: Get a dictionary mapping choice name to value in a SkrubLearner.

[`SkrubLearner.set_named_params`](skrub.SkrubLearner.md#skrub.SkrubLearner.set_named_params)
: Set choice values by name on a SkrubLearner.

### Examples

```pycon
>>> import skrub
>>> dop = (
...     skrub.var("a")
...     + skrub.choose_int(0, 10, name="b")
...     + skrub.choose_float(0.0, 1.0)
... )
>>> dop.skb.get_choices()
{'b': choose_int(0, 10, name='b'), 'choose_float(0.0, 1.0)': choose_float(0.0, 1.0)}
```

With `named_only` we can restrict to choices that have an actual name
(only choices with a name can be passed in an environment, and to
[`SkrubLearner.set_named_params()`](skrub.SkrubLearner.md#skrub.SkrubLearner.set_named_params)):

```pycon
>>> dop.skb.get_choices(named_only=True)
{'b': choose_int(0, 10, name='b')}
```

Similarly, vars can be obtained:

```pycon
>>> dop.skb.get_vars()
{'a': <Var 'a'>}
```

All the names that can be passed in the environment are thus obtained with:

```pycon
>>> list(dop.skb.get_vars(all_named_ops=True) | dop.skb.get_choices(named_only=True))
['a', 'b']
```

<!-- !! processed by numpydoc !! -->
