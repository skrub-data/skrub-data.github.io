# eval_mode

### skrub.eval_mode()

Return the mode in which the DataOp is currently being evaluated.

This can be:

- ‘preview’: when the previews are being eagerly computed when the
  DataOp is defined or when we call [`DataOp.skb.eval()`](skrub.DataOp.skb.evalhtml.md#skrub.DataOp.skb.eval) without
  arguments.
- otherwise, the method we called on the learner such as `'predict'`
  or `'fit_transform'`.

### Examples

```pycon
>>> import skrub
```

```pycon
>>> mode = skrub.eval_mode()
>>> mode.skb.preview()
'preview'
>>> mode.skb.eval()
'fit_transform'
>>> learner = mode.skb.make_learner()
>>> learner.fit_transform({})
'fit_transform'
>>> learner.transform({})
'transform'
```

`eval_mode()` can be particularly useful to have a different behavior in
preview mode in order to speed-up previews during debugging:

```pycon
>>> n_components = skrub.eval_mode().skb.match({'preview': 2}, default=20)
>>> n_components
<Match <EvalMode>>
Result:
―――――――
2
```

<!-- !! processed by numpydoc !! -->
