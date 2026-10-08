# skrub.DataOp.skb.draw_graph

#### DataOp.skb.draw_graph(, show_ids=False)

Get an SVG string representing the computation graph.

In addition to the usual `str` methods, the result has an `open()`
method which displays it in a web browser window.

* **Returns:**
  GraphDrawing
  : Drawing of the computation graph. This objects has attributes
    `svg` and `png`, containing representations of the graph in
    those formats (as `bytes` objects), and a method `.open()` to
    display it in a browser window.

<!-- !! processed by numpydoc !! -->
