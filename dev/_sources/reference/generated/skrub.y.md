# y

### skrub.y(value=NULL)

Create a skrub variable and mark it as being `y`.

This is just a convenient shortcut for:

```default
skrub.var("y", value).skb.mark_as_y()
```

Marking a variable as `y` tells skrub that this is the column or table of
targets that must be split into training and testing sets for
cross-validation. Please refer to the examples gallery for more
information.

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
[`skrub.X`](skrub.Xhtml.md#skrub.X)
: Create a skrub variable and mark it as being `y`.

[`skrub.var`](skrub.varhtml.md#skrub.var)
: Create a skrub variable.

[`skrub.DataOp.skb.mark_as_y`](skrub.DataOp.skb.mark_as_yhtml.md#skrub.DataOp.skb.mark_as_y)
: Mark this DataOp as being the `y` table.

### Examples

```pycon
>>> import skrub
>>> col = skrub.datasets.toy_orders().delayed
>>> y = skrub.y(col)
>>> y
<Var 'y'>
Result:
―――――――
0    False
1    False
2     True
3    False
Name: delayed, dtype: bool
>>> y.skb.name
'y'
>>> y.skb.is_y
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
