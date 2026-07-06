# deferred

### skrub.deferred(func)

Wrap function calls in a DataOp [`DataOp`](skrub.DataOphtml.md#skrub.DataOp).

When this decorator is applied, the resulting function returns DataOps.
The returned DataOp wraps the call to the original function, and the
call is executed when the DataOp is evaluated. This allows including calls
to any function as a step in a learner, rather than executing it immediately.

See the examples gallery for an in-depth explanation of skrub DataOps
and `deferred`.

* **Parameters:**
  **func**
  : The function to wrap
* **Returns:**
  A new function
  : When called, rather than applying the original function immediately, it
    returns a DataOp. Evaluating the DataOp applies the original
    function.

#### SEE ALSO
[`as_data_op`](skrub.as_data_ophtml.md#skrub.as_data_op)
: Create a DataOp that evaluates to the given value.

[`DataOp`](skrub.DataOphtml.md#skrub.DataOp)
: Representation of a computation that can be used to build ML estimators.

### Examples

```pycon
>>> def tokenize(text):
...     words = text.split()
...     return [w for w in words if w not in ['the', 'of']]
>>> tokenize('the first day of the week')
['first', 'day', 'week']
```

```pycon
>>> import skrub
>>> text = skrub.var('text')
```

Calling `tokenize` on a skrub DataOp raises an exception:
`tokenize` tries to iterate immediately over the tokens to remove stop
words, but the text will only be known when we run the learner.

```pycon
>>> tokens = tokenize(text)
Traceback (most recent call last):
    ...
TypeError: This object is a DataOp that will be evaluated later, when your learner runs. So it is not possible to eagerly iterate over it now.
```

We can defer the call to `tokenize` until we are evaluating the
DataOp:

```pycon
>>> tokens = skrub.deferred(tokenize)(text)
>>> tokens
<Call 'tokenize'>
>>> tokens.skb.eval({'text': 'the first month of the year'})
['first', 'month', 'year']
```

Like any decorator `deferred` can be called explicitly as shown above or
used with the `@` syntax:

```pycon
>>> @skrub.deferred
... def log(x):
...     print('INFO x =', x)
...     return x
>>> x = skrub.var('x')
>>> e = log(x)
>>> e.skb.eval({'x': 3})
INFO x = 3
3
```

**Advanced examples**

As we saw in the last example above, the arguments passed to the function,
if they are DataOps, are evaluated before calling it. This is also the
case for global variables, default arguments and free variables.

```pycon
>>> a = skrub.var('a')
>>> b = skrub.var('b')
>>> c = skrub.var('c')
```

```pycon
>>> @skrub.deferred
... def f(x, y=b):
...     z = c
...     print(f'{x=}, {y=}, {z=}')
...     return x + y + z
```

```pycon
>>> result = f(a)
>>> result
<Call 'f'>
>>> result.skb.eval({'a': 100, 'b': 20, 'c': 3})
x=100, y=20, z=3
123
```

Another example with a closure:

```pycon
>>> import numpy as np
```

```pycon
>>> def make_transformer(mode, period):
...
...     @skrub.deferred
...     def transform(x):
...         if mode == "identity":
...             return x[:, None]
...         assert mode == "trigo", mode
...         x = x / period * 2 * np.pi
...         return np.asarray([np.sin(x), np.cos(x)]).T.round(2)
...
...     return transform
```

```pycon
>>> hour = skrub.var("hour")
>>> hour_encoding = skrub.choose_from(["identity", "trigo"], name="hour_encoding")
>>> transformer = make_transformer(hour_encoding, 24)
>>> out = transformer(hour)
```

The free variable `mode` is evaluated before calling the deferred (inner)
function so `transform` works as expected:

```pycon
>>> out.skb.eval({"hour": np.arange(0, 25, 4)})
array([[ 0],
       [ 4],
       [ 8],
       [12],
       [16],
       [20],
       [24]])
>>> out.skb.eval({"hour": np.arange(0, 25, 4), "hour_encoding": "trigo"})
array([[ 0.  ,  1.  ],
       [ 0.87,  0.5 ],
       [ 0.87, -0.5 ],
       [ 0.  , -1.  ],
       [-0.87, -0.5 ],
       [-0.87,  0.5 ],
       [-0.  ,  1.  ]])
```

<!-- !! processed by numpydoc !! -->
