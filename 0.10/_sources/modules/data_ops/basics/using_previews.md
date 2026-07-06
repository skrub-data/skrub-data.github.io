<a id="user-guide-data-ops-using-previews"></a>

# Using previews for easier development and debugging

To make interactive development easier without having to call `eval()` after
each step, it is possible to preview the result of a DataOp by passing a value
along with its name when creating a variable.

```pycon
>>> import skrub
>>> a = skrub.var("a", 10) # we pass the value 10 in addition to the name
>>> b = skrub.var("b", 6)
>>> c = a + b
>>> c  # now the display of c includes a preview of the result
<BinOp: add>
Result:
―――――――
16
```

Previews are eager computations on the current data, and since they are computed
immediately they can spot errors early on:

```pycon
>>> import pandas as pd
>>> df = pd.DataFrame({"col": [1, 2, 3]})
>>> a = skrub.var("a", df)  # we pass the DataFrame as a value
```

Next, we use the pandas `drop` column and try to drop a column without
specifying the axis:

```pycon
>>> a.drop("col")
Traceback (most recent call last):
    ...
RuntimeError: Evaluation of '.drop()' failed.
You can see the full traceback above. The error message was:
KeyError: "['col'] not found in axis"
```

Note that seeing results for the values we provided does *not* change the fact
that we are building a pipeline that we want to reuse, not just computing the
result for a fixed input. The displayed result is only preview of the output on
one example dataset.

```pycon
>>> c.skb.eval({"a": 3, "b": 2})
5
```

It is not necessary to provide a value for every variable: it is however advisable
to do so when possible, as it allows to catch errors early on.

Note: you can obtain the preview values with [`DataOp.skb.get_data()`](../../../reference/generated/skrub.DataOp.skb.get_datahtml.md#skrub.DataOp.skb.get_data), and
set different ones with [`DataOp.skb.set_data()`](../../../reference/generated/skrub.DataOp.skb.set_datahtml.md#skrub.DataOp.skb.set_data).

## Defining a default value for a variable

If we pass `becomes_default=True` to [`var()`](../../../reference/generated/skrub.varhtml.md#skrub.var), the provided `value` is not
only an example value to use for previews but a default value for this variable
in all contexts – then it is always optional to pass a value for it in the
environment, and if not found the default is used.

```pycon
>>> a = skrub.var('a', 0)
>>> a
<Var 'a'>
Result:
―――――――
0
>>> b = skrub.var('b', 1, becomes_default=True)
>>> b
<Var 'b' int>
Result (also the default value):
――――――――――――――――――――――――――――――――
1
>>> c = a + b
>>> c.skb.eval({'a': 10}) # the default 1 is used for 'b'
11
```

See the documentation of [`var()`](../../../reference/generated/skrub.varhtml.md#skrub.var) for details.

## Disabling previews and eager checks

By default, as soon as a DataOp is defined, some validity checks are performed
and the preview results are computed eagerly. In very complex DataOps plans
(100+ nodes), running checks after adding each node can cause a noticeable overhead.
To avoid this, it is possible to disable eager checks with the `"eager_data_ops"`
is easily achieved with the `"eager_data_ops"` [configuration](../../../guides/utilities/customizing_configurationhtml.md#user-guide-configuration-parameters) option.

```pycon
>>> with skrub.config_context(eager_data_ops=False):
...     # no checks are performed when b is defined so no error in the line below:
...     b = skrub.var('a', 1) + skrub.var('a', 2)
...     # checks are still performed (once) before the DataOp is actually used so
...     # evaluating the DataOp, using .skb.make_learner() etc _would_ still raise:
...     # b.skb.eval() ## raises ValueError: Choice and node names must be unique.
>>> b # Note there is no preview, even though we provided values for the variables
<BinOp: add>
```
