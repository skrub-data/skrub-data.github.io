# make_selector

### skrub.selectors.make_selector(obj)

Transform a selector, column name or list of column names into a selector.

### Examples

```pycon
>>> from skrub import selectors as s
```

```pycon
>>> s.make_selector('ID')
cols('ID')
```

```pycon
>>> s.make_selector(['ID', 'kind'])
cols('ID', 'kind')
```

```pycon
>>> s.make_selector(s.cols('ID', 'kind'))
cols('ID', 'kind')
```

<!-- !! processed by numpydoc !! -->
