# has_dtype

### skrub.selectors.has_dtype(\*dtypes)

Select columns whose dtype is equal to one of the provided dtypes.

This is an advanced selector for edge cases where you need to match specific
dtypes not covered by other selectors. Use this when working with specialized
or custom dtypes (e.g., pandas ListDtype, polars Object).

For standard types, prefer the simpler selectors like [`numeric()`](skrub.selectors.numerichtml.md#skrub.selectors.numeric),
[`string()`](skrub.selectors.stringhtml.md#skrub.selectors.string), [`categorical()`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical), or [`boolean()`](skrub.selectors.booleanhtml.md#skrub.selectors.boolean).

This selector takes a hands-off approach: skrub does not normalize or infer
dtypes across dataframe libraries. A column is selected if
`sbd.dtype(column) == dtype` for at least one of the provided `dtypes`.

* **Parameters:**
  **\*dtypes**
  : One or more dtype objects to match.

#### SEE ALSO
[`numeric`](skrub.selectors.numerichtml.md#skrub.selectors.numeric)
: Select numeric columns.

[`string`](skrub.selectors.stringhtml.md#skrub.selectors.string)
: Select string columns.

[`categorical`](skrub.selectors.categoricalhtml.md#skrub.selectors.categorical)
: Select categorical columns.

[`object`](skrub.selectors.objecthtml.md#skrub.selectors.object)
: Select columns with “object” dtype (library specific).

### Notes

Some dataframe libraries may accept shorthand values that compare equal to a
dtype object (e.g., ‘int64’), but this is backend-specific. For robustness,
pass dtype objects obtained from your dataframe library.

### Examples

```pycon
>>> from skrub import selectors as s
>>> import pandas as pd
>>> df = pd.DataFrame(
...     {
...         "items": [["A4", "A3"], ["A5"]],
...         "count": [2, 1],
...     }
... )
```

Get dtype from an existing column and use it for selection:

```pycon
>>> s.select(df, s.has_dtype(df["items"].dtype))
       items
0  [A4, A3]
1      [A5]
```

Match multiple dtypes at once:

```pycon
>>> items_dtype = df["items"].dtype
>>> count_dtype = df["count"].dtype
>>> s.select(df, s.has_dtype(items_dtype, count_dtype))
       items  count
0  [A4, A3]      2
1      [A5]      1
```

This also works with complex dtypes such as GeometryDtype in GeoPandas:

```pycon
>>> import geopandas as gpd
>>> from shapely.geometry import Point
>>> gdf = gpd.GeoDataFrame(
...     {"city": ["Paris", "Berlin"], "value": [1, 2]},
...     geometry=[Point(2.35, 48.86), Point(13.40, 52.52)],
...     crs="EPSG:4326",
... )
>>> gdf
    city  value            geometry
0   Paris      1  POINT (2.35 48.86)
1  Berlin      2  POINT (13.4 52.52)
>>> s.select(gdf, s.has_dtype(gdf.geometry.dtype))
            geometry
0  POINT (2.35 48.86)
1  POINT (13.4 52.52)
```

<!-- !! processed by numpydoc !! -->
