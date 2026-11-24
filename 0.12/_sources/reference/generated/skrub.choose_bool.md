# choose_bool

### skrub.choose_bool(, name=None, default=True)

A choice between `True` and `False`.

When a learner is fitted *without hyperparameter tuning*, the outcome of
this choice is `True`. Pass `default=False` to make `False` the
default outcome.

* **Parameters:**
  **name**
  : If not `None`, `name` is used when displaying search results and
    can also be used to override the choice’s value by setting it in the
    environment containing a learner’s inputs.

  **default**
  : Choice’s default value when hyperparameter search is not used.
* **Returns:**
  BoolChoice
  : An object representing this choice, which can be used in a skrub
    learner.

#### SEE ALSO
[`choose_float`](skrub.choose_floathtml.md#skrub.choose_float)
: Construct a choice of floating-point numbers from a numeric range.

[`choose_from`](skrub.choose_fromhtml.md#skrub.choose_from)
: Construct a choice among several possible outcomes.

[`choose_int`](skrub.choose_inthtml.md#skrub.choose_int)
: Construct a choice of integers from a numeric range.

[`optional`](skrub.optionalhtml.md#skrub.optional)
: Choose between executing an operation or not.

### Examples

```pycon
>>> import skrub
>>> print(skrub.choose_bool().as_data_op().skb.describe_param_grid())
- choose_bool(): [True, False]
>>> skrub.choose_bool().default()
True
```

We can set the default to make it `False`:

```pycon
>>> skrub.choose_bool(default=False).default()
False
```

<!-- !! processed by numpydoc !! -->
