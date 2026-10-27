# get_config

### skrub.get_config()

Retrieve current values for configuration set by [`set_config()`](skrub.set_confightml.md#skrub.set_config).

* **Returns:**
  **config**
  : Keys are parameter names that can be passed to [`set_config()`](skrub.set_confightml.md#skrub.set_config).

#### SEE ALSO
[`config_context`](skrub.config_contexthtml.md#skrub.config_context)
: Context manager for global skrub configuration.

[`set_config`](skrub.set_confightml.md#skrub.set_config)
: Set global skrub configuration.

### Examples

```pycon
>>> import skrub
>>> config = skrub.get_config()
>>> config.keys()
dict_keys([...])
```

<!-- !! processed by numpydoc !! -->
