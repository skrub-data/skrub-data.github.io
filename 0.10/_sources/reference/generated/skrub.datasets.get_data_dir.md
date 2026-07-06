# get_data_dir

### skrub.datasets.get_data_dir(name=None, data_home=None)

Returns the directory in which skrub looks for data.

This is typically useful for the end-user to check
where the data is downloaded and stored.

* **Parameters:**
  **name**
  : Subdirectory name. If omitted, the root data directory is returned.

  **data_home**
  : The path to skrub data directory. If `None`, the default path
    is `~/skrub_data`.

<!-- !! processed by numpydoc !! -->
