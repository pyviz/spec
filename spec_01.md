- Spec: 1
- Title: Methods on libraries
- Version: 0.1
- Last-Modified: 2019-07-19
- Authors: [James Bednar](),
           [Julia Signell](jsignell@gmail.com),
           [Jake Vanderplas]()
- Status: Active
- Type: Standards
- Content-Type: text/markdown
- Created: 2019-07-19

## Abstract
This spec describes a proposed minimal shared Python API for plotting libraries.

This spec uses LIBRARY to refer to any specific visualization library.

## If a method doesn't make sense
If a given operation doesn't make sense for that library, then it can satisfy the spec by simply having that method return a message to that effect ("Unsupported: LIBRARY does not provide JSON output").

## `.__pyviz_spec__()` method
Calling `.__pyviz_spec__()` on the library should return a list of the specs that the library complies with. It should include the version of the spec that is being used.

```python
>>> LIBRARY.__spec_version__()
{1: 0.1, 2: 0.1}
```

## Enable output from library
Each library should provide a uniform way to enable itself in jupyter notebook & jupyterlab.

> #### What currently exists
> - matplotlib has `%matplotlib inline`
> - bokeh has `bokeh.output_notebook()`
> - altair has `alt.renderers.enable('notebook')`
> - holoviews has `hv.extension('bokeh')`
> - hvplot has `import hvplot.pandas`

### Proposal: function call
Provide an `enable()` function call on the library that can take an optional `output` kwarg, but provides a sensible default.

`LIBRARY.enable()` == `LIBRARY.enable(output='notebook')`

**NOTE**: This spec doesn't cover the case where you are in a notebook and want to generate a figure separately from a notebook.

#### Context
A function call is preferred over magics because some tools need a specification that is usable both within and outside of jupyter. We can always add special cases to deal with magics, but prefer just to have a normal Python call that can register things with Jupyter if it's available but doesn't otherwise cause syntax errors (if not skipped) or missing functionality (if skipped) outside of Jupyter/IPython.
