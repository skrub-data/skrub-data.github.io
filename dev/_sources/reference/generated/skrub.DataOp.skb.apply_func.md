# skrub.DataOp.skb.apply_func

#### DataOp.skb.apply_func(func, \*args, \*\*kwargs)

Apply the given function.

This is a convenience function; `X.skb.apply_func(func)` is
equivalent to `skrub.deferred(func)(X)`.

* **Parameters:**
  **func**
  : The function to apply to the DataOp.

  **args**
  : additional positional arguments passed to `func`.

  **kwargs**
  : named arguments passed to `func`.
* **Returns:**
  data_op
  : The DataOp that evaluates to the result of calling `func` as
    `func(self, *args, **kwargs)`.

### Examples

```pycon
>>> import skrub
```

```pycon
>>> def count_words(text, sep=None):
...     return len(text.split(sep))
```

```pycon
>>> text = skrub.var("text", "Hello, world!")
>>> text
<Var 'text'>
Result:
―――――――
'Hello, world!'
```

```pycon
>>> count = text.skb.apply_func(count_words)
>>> count
<Call 'count_words'>
Result:
―――――――
2
>>> count.skb.eval({"text": "one two three four"})
4
```

We can pass extra arguments:

```pycon
>>> text.skb.apply_func(count_words, sep="\n")
<Call 'count_words'>
Result:
―――――――
1
```

Using `.skb.apply_func` is the same as using `deferred`, for example:

```pycon
>>> skrub.deferred(count_words)(text)
<Call 'count_words'>
Result:
―――――――
2
```

<!-- !! processed by numpydoc !! -->
