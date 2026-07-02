<a id="user-guide-data-ops-control-flow"></a>

# Control flow in DataOps: eager and deferred evaluation

DataOps represent computations that have not been executed yet, and will
only be triggered when we call [`.skb.eval()`](../../../reference/generated/skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval), or when we
create the pipeline with [`.skb.make_learner()`](../../../reference/generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner) and
call one of its methods such as `fit()`.

This means we cannot use standard Python control flow statements such as `if`,
`for`, `with`, etc. with DataOps, because those constructs would execute
immediately.

```pycon
>>> import pandas as pd
>>> import skrub
>>> orders_df = pd.DataFrame(
...     {
...         "item": ["pen", "cup", "pen", "fork"],
...         "price": [1.5, None, 1.5, 2.2],
...         "qty": [1, 1, 2, 4],
...     }
... )
>>> orders = skrub.var("orders", orders_df)
>>> for column in orders.columns:
...     pass
Traceback (most recent call last):
    ...
TypeError: This object is a DataOp that will be evaluated later, when your learner runs. So it is not possible to eagerly iterate over it now.
```

We get an error because the `for` statement tries to iterate immediately
over the columns. However, `orders.columns` is not an actual list of
columns: it is a skrub DataOp that will produce a list of columns, later,
when we run the computation.

This remains true even if we have provided a value for `orders` and we can
see a result for that value:

```pycon
>>> orders.columns
<GetAttr 'columns'>
Result:
―――――――
Index(['item', 'price', 'qty'], dtype=...)
```

The “result” we see is an *example* result that the computation produces for the
data we provided. But we want to fit our pipeline and apply it to different
datasets, for which it will return a new object every time. So even if we see a
preview of the output on the data we provided, `orders.columns` still
represents a future computation that remains to be evaluated.

Therefore, we must delay the execution of the `for` statement until the computation
actually runs and `orders.columns` has been evaluated.

We can achieve this by defining a function that contains the control flow logic
we need, and decorating it with [`deferred()`](../../../reference/generated/skrub.deferredhtml.md#skrub.deferred). This decorator defers the execution
of the function: when we call it, it does not run immediately. Instead, it returns
a skrub DataOp that wraps the function call. The original function is only
executed when the DataOp is evaluated, and will return the result as a DataOp.

```pycon
>>> @skrub.deferred
... def with_upper_columns(df):
...     new_columns = [c.upper() for c in df.columns]
...     return df.set_axis(new_columns, axis="columns")
```

```pycon
>>> with_upper_columns(orders)
<Call 'with_upper_columns'>
Result:
―――――――
   ITEM  PRICE  QTY
0   pen    1.5    1
1   cup    NaN    1
2   pen    1.5    2
3  fork    2.2    4
```

When the computation runs, `orders` will be evaluated first and the result (an
actual dataframe) will be passed as the `df` argument to our function. In practice,
the code inside a deferred function is completely equivalent to eager code, so
it is possible to use any Python control flow statement inside it, as well as
act on the data as if it were a regular DataFrame.

Within a function decorated with [`deferred()`](../../../reference/generated/skrub.deferredhtml.md#skrub.deferred), objects are evaluated eagerly,
so it is possible to use standard Python control flow statements such as
`if`, `for`, and it is possible to treat the inputs as if they were
regular objects (e.g., a Pandas DataFrame or Series).

When the first argument to our function is a skrub DataOp, rather than
applying `deferred` and calling the function as shown above we can use
[`.skb.apply_func()`](../../../reference/generated/skrub.DataOp.skb.apply_funchtml.md#skrub.DataOp.skb.apply_func):

```pycon
>>> def with_upper_columns(df):
...     new_columns = [c.upper() for c in df.columns]
...     return df.set_axis(new_columns, axis="columns")
```

```pycon
>>> orders.skb.apply_func(with_upper_columns)
<Call 'with_upper_columns'>
Result:
―――――――
   ITEM  PRICE  QTY
0   pen    1.5    1
1   cup    NaN    1
2   pen    1.5    2
3  fork    2.2    4
```

## Unpacking multiple outputs from deferred functions

When a deferred function returns more than one value, you cannot unpack the
result directly because unpacking iterates over the result. Iteration is not
supported on DataOps until evaluation.

In general, it is recommended that deferred functions return a single
value whenever possible. Returning multiple outputs should be avoided unless
strictly necessary, as it makes downstream usage more complex.

Instead, keep the result as a single DataOp and index into it:

```pycon
>>> test = skrub.var("test", [1, 2])
>>> @skrub.deferred
... def process_test_data(test):
...     left = test[0]
...     right = test[1]
...     return left, right
>>> res = test.skb.apply_func(process_test_data)
>>> left = res[0]
>>> right = res[1]
```

[`deferred()`](../../../reference/generated/skrub.deferredhtml.md#skrub.deferred) is useful not only for our own functions, but also when we
need to call module-level functions from a library. For example, to delay the
loading of a CSV file, we could write something like:

```pycon
>>> csv_path = skrub.var("csv_path")
>>> data = skrub.deferred(pd.read_csv)(csv_path)
```

or, with `apply_func`:

```pycon
>>> data = csv_path.skb.apply_func(pd.read_csv)
```

Another consequence of the fact that DataOps are evaluated lazily (we are
building a pipeline, not immediately computing a single result), any
transformation that we apply must not modify its input, but leave it unchanged
and return a new value.

Consider the transformers in a scikit-learn pipeline: each computes a new
result without modifying its input.

```pycon
>>> orders['total'] = orders['price'] * orders['qty']
Traceback (most recent call last):
    ...
TypeError: Do not modify a DataOp in-place. Instead, use a function that returns a new value. This is necessary to allow chaining several steps in a sequence of transformations.
For example if df is a pandas DataFrame:
df = df.assign(new_col=...) instead of df['new_col'] = ...
```

Note the suggestion in the error message: using [`pandas.DataFrame.assign()`](http://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.assign.html#pandas.DataFrame.assign).
When we do need assignments or in-place transformations, we can put them in a
[`deferred()`](../../../reference/generated/skrub.deferredhtml.md#skrub.deferred) function. But we should make a (shallow) copy of the inputs and
return a new value.

Finally, there are other situations where using [`deferred()`](../../../reference/generated/skrub.deferredhtml.md#skrub.deferred) can be helpful:

- When we have many nodes in our graph and want to collapse a sequence of steps into
  a single function call that appears as a single node.
- When certain function calls need to be deferred until the full computation
  runs, because they depend on the runtime environment, or on objects that
  cannot be pickled with the rest of the computation graph (for example, opening
  and reading a file).

### Examples

- See sphx_glr_auto_examples_data_ops_1111_data_ops_quick_tour.py for an introductory
  example on how to use skrub DataOps on a single dataframe.
- See [Multiples tables: building machine learning pipelines with DataOps](../../../auto_examples/02_data_ops/1120_multiple_tableshtml.md#sphx-glr-auto-examples-02-data-ops-1120-multiple-tables-py) for an example
  of how skrub DataOps can be used to process multiple tables using dataframe APIs.
- See [Hyperparameter tuning with DataOps](../../../auto_examples/02_data_ops/1130_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py) for an example of
  hyper-parameter tuning using skrub DataOps.
