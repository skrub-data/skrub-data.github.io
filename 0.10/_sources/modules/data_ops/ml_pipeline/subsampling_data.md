<a id="user-guide-data-ops-subsampling"></a>

# Subsampling data for easier development and debugging

If the data used for the preview is large, it can be useful to work on a
subsample of the data to speed up the development and debugging process.
This can be done by calling the [`.skb.subsample()`](../../../reference/generated/skrub.DataOp.skb.subsamplehtml.md#skrub.DataOp.skb.subsample) method
on a variable: this signals to skrub that what is shown when printing DataOps, or
returned by [`.skb.preview()`](../../../reference/generated/skrub.DataOp.skb.previewhtml.md#skrub.DataOp.skb.preview) is computed on a subsample
of the data.

Note that subsampling is “local”: if it is applied to a variable, it only
affects the variable itself. This may lead to unexpected results and errors
if, for example, `X` is subsampled but `y` is not.

Subsampling **is turned off** by default when we call other methods such as
[`.skb.eval()`](../../../reference/generated/skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval),
[`.skb.cross_validate()`](../../../reference/generated/skrub.DataOp.skb.cross_validatehtml.md#skrub.DataOp.skb.cross_validate),
[`.skb.train_test_split`](../../../reference/generated/skrub.DataOp.skb.train_test_splithtml.md#skrub.DataOp.skb.train_test_split),
[`DataOp.skb.make_learner()`](../../../reference/generated/skrub.DataOp.skb.make_learnerhtml.md#skrub.DataOp.skb.make_learner),
[`DataOp.skb.make_randomized_search()`](../../../reference/generated/skrub.DataOp.skb.make_randomized_searchhtml.md#skrub.DataOp.skb.make_randomized_search), etc.
However, all of those methods have a `keep_subsampling` parameter that we can
set to `True` to force using the subsampling when we call them. Note that
even if we set `keep_subsampling=True`, subsampling is not applied when using
`predict`.

See more details in a [full example](../../../auto_examples/02_data_ops/1140_subsamplinghtml.md#sphx-glr-auto-examples-02-data-ops-1140-subsampling-py).
