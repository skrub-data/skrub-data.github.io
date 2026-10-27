# make_selector

### skrub.selectors.make_selector(obj)

Normalize a selector, column name, or list of names into a `Selector`    object.

This function converts user input into a consistent selector object:

- Selectors are returned as-is
- Strings are converted to `cols(name)`
- Lists of strings are converted to `cols(*names)`

* **Parameters:**
  **obj**
  : The object to normalize.
* **Returns:**
  Selector
  : A `Selector` object (or subclass).

#### SEE ALSO
[`cols`](skrub.selectors.colshtml.md#skrub.selectors.cols)
: Select specific columns by name

### Examples

This function is used to normalize user input so that the result is a
selector compatible with the rest of the API.

```pycon
>>> from skrub import selectors as s
```

Normalize a column name or list of column names:

```pycon
>>> s.make_selector('ID')
cols('ID')
>>> s.make_selector(['ID', 'kind'])
cols('ID', 'kind')
```

A selector is returned unchanged:

```pycon
>>> s.make_selector(s.cols('ID', 'kind'))
cols('ID', 'kind')
```

<!-- !! processed by numpydoc !! -->
