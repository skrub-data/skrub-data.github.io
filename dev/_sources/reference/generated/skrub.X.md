# X

### skrub.X(value=NULL)

Create a skrub variable and mark it as being `X`.

This is just a convenient shortcut for:

```default
skrub.var("X", value).skb.mark_as_X()
```

Marking a variable as `X` tells skrub that this is the design matrix that
must be split into training and testing sets for cross-validation. Please
refer to the examples gallery for more information.

* **Parameters:**
  **value**
  : The value passed to [`skrub.var()`](skrub.varhtml.md#skrub.var), which is used for previews of the
    learner’s outputs, cross-validation etc. as described in the
    documentation for [`skrub.var()`](skrub.varhtml.md#skrub.var) and the examples gallery.
* **Returns:**
  A skrub variable
* **Raises:**
  TypeError
  : If the provided value is a skrub DataOp or a skrub choose_\* function.

#### SEE ALSO
[`skrub.y`](skrub.yhtml.md#skrub.y)
: Create a skrub variable and mark it as being `y`.

[`skrub.var`](skrub.varhtml.md#skrub.var)
: Create a skrub variable.

[`skrub.DataOp.skb.mark_as_X`](skrub.DataOp.skb.mark_as_Xhtml.md#skrub.DataOp.skb.mark_as_X)
: Mark this DataOp as being the `X` table.

### Examples

```pycon
>>> import skrub
>>> df = skrub.datasets.toy_orders().orders_
>>> X = skrub.X(df)
>>> X
<Var 'X'>
Result:
―――――――
   ID product  quantity        date
0   1     pen         2  2020-04-03
1   2     cup         3  2020-04-04
2   3     cup         5  2020-04-04
3   4   spoon         1  2020-04-05
>>> X.skb.name
'X'
>>> X.skb.is_X
True
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Use case: developing locally and deploying to production">  <div class="sphx-glr-thumbnail-title">Use case: developing locally and deploying to production</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to wrap a PyTorch model with skorch and plug it into a skrub DataOps plan.">  <div class="sphx-glr-thumbnail-title">Using PyTorch (via skorch) in DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>
