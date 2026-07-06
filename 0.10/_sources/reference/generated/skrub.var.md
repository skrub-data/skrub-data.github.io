# var

### skrub.var(name, value=NULL, , becomes_default=False)

Create a skrub variable.

Variables represent inputs to a DataOps plan, and the corresponding learner.
They can be combined with other variables, constants, operators, function
calls etc. to build up complex DataOps, which implicitly define the plan.

See the example gallery for more information about skrub DataOps.

* **Parameters:**
  **name**
  : The name for this input. It corresponds to a key in the dictionary that
    is passed to the learner’s `fit()` method (see Examples below).
    Names must be unique within a learner and must not start with
    `"_skrub_"`

  **value**
  : Optionally, an initial value can be given to the variable. When it is
    available, it is used to provide a preview of the learner’s results,
    to detect errors in the learner early, and to provide better help and
    tab-completion in interactive Python shells.

  **becomes_default**
  : If True, the provided `value` is not only used for previews but also
    becomes the default value for this variable when creating a learner
    (for example with [`DataOp.skb.make_learner()`](skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner)). Thus passing this
    variable in the environment is always optional.
* **Returns:**
  A skrub variable
* **Raises:**
  TypeError
  : If the provided value is a skrub DataOp or a skrub choose_\* function.

#### SEE ALSO
[`skrub.X`](skrub.Xhtml.md#skrub.X)
: Create a skrub variable and mark it as being `X`.

[`skrub.y`](skrub.yhtml.md#skrub.y)
: Create a skrub variable and mark it as being `y`.

### Examples

Variables without a value:

```pycon
>>> import skrub
>>> a = skrub.var('a')
>>> a
<Var 'a'>
>>> b = skrub.var('b')
>>> c = a + b
>>> c
<BinOp: add>
>>> print(c.skb.describe_steps())
Var 'a'
Var 'b'
BinOp: add
```

The names of variables correspond to keys in the inputs:

```pycon
>>> c.skb.eval({'a': 10, 'b': 6})
16
```

And also to keys to the inputs to the DataOps plan:

```pycon
>>> learner = c.skb.make_learner()
>>> learner.fit_transform({'a': 5, 'b': 4})
9
```

When providing a value, we see what the learner produces for the values we
provided:

```pycon
>>> a = skrub.var('a', 2)
>>> b = skrub.var('b', 3)
>>> b
<Var 'b'>
Result:
―――――――
3
>>> c = a + b
>>> c
<BinOp: add>
Result:
―――――――
5
```

The values are also used for `eval()` when no environment is provided:

```pycon
>>> c.skb.eval()
5
```

But we can still override them. And inputs must be provided explicitly when
using the learner returned by `.skb.make_learner()`.

```pycon
>>> c.skb.eval({'a': 10, 'b': 6})
16
```

When passing `becomes_default=True`, the preview value is treated as a
default value for that variable. It is kept when cloning the DataOp or
creating a Learner, and is always optional (does not need to be present) in
the provided environment.

```pycon
>>> c = skrub.var('a', 0, becomes_default=True) + skrub.var('b', 1)
>>> c.skb.get_data()
{'a': 0, 'b': 1}
>>> c.skb.clone().skb.get_data()
{'a': 0}
```

For the learner ‘b’ is mandatory but ‘a’ is optional and has default value
0.

```pycon
>>> c.skb.make_learner().fit_transform({'b': 10})
10
>>> c.skb.make_learner().fit_transform({'b': 10, 'a': 100})
110
```

Much more information about skrub variables is provided in the examples
gallery.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we give a bird&#x27;s eye view of the DataOps workflow on a simple regression task that we saw in an early example &lt;example_encodings&gt;: predicting the salaries of US Government employees.">  <div class="sphx-glr-thumbnail-title">Quick overview of DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to use DataOp.skb.subsample to speed up interactive construction of a skrub DataOps plan by computing previews on a subsampled version of the original data.">  <div class="sphx-glr-thumbnail-title">Subsampling for faster development</div>
</div>
<!-- thumbnail-parent-div-close --></div>
