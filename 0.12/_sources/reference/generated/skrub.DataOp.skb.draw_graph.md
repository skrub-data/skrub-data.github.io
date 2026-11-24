# skrub.DataOp.skb.draw_graph

#### DataOp.skb.draw_graph(, show_ids=False)

Get a drawing of the computation graph.

In addition to the usual `str` methods, the result has an `open()`
method which displays it in a web browser window.

* **Returns:**
  GraphDrawing
  : Drawing of the computation graph. This objects has attributes
    `svg` and `png`, containing representations of the graph in
    those formats (as `bytes` objects), and a method `.open()` to
    display it in a browser window.

### Notes

The graph is drawn by [Graphviz](https://graphviz.org/download/),
which must be installed on the system with its package manager (not
with pip). If it is not installed, the `svg` and `png` attributes
raise an error, but the graph is still displayed by `open()` and in
notebooks: it is then drawn by the web browser, so JavaScript must be
enabled. In a notebook, the browser needs an internet connection to
download the library that draws the graph.

<!-- !! processed by numpydoc !! -->
