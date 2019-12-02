- Spec: 2
- Title: Methods on figure objects
- Version: 0.1
- Last-Modified: 2019-07-19
- Authors: [James Bednar](),
           [David Hoese](),
           [Jon Mease](jon.mease@gmail.com),
           [Julia Signell](jsignell@gmail.com),
- Status: Active
- Type: Standards
- Content-Type: text/markdown
- Created: 2019-07-19

## Abstract
This spec describes a proposed shared Python API for accessing objects from various plotting libraries.

In this notebook we will use **figure** to refer to any object that has a visible representation but that can also be saved, exported, etc. This includes generic visualizations or animations that may or may not be considered a "plot" with axes, ticks, and so on.```

When detailing a method, this spec uses FIGURE to refer to any specific figure object and LIBRARY to refer to any specific visualization library.

## If a method doesn't make sense
If a given operation doesn't make sense for that library, then it can satisfy the spec by simply having that method return a message to that effect ("Unsupported: LIBRARY does not provide JSON output").

## Jupyter methods
Every library that supports rendering in Jupyter should support at least one of the various IPython rich display methods, i.e. `_repr_html_`, `_repr_png_`, `_ipython_display_`.

## `.show()`
Every figure should have the ability to render itself.

### Proposal
 1) The top-level figure class should have a `.show()` method that can be called without arguments to display the figure as a side-effect.
 2) The optional `renderer` kwarg can be used to override the current default renderer. e.g.:
    - `FIGURE.show(renderer='png')` to display the figure as a static png image.
    - `FIGURE.show(renderer='browser')` to display the figure in a new browser tab. This works in both jupyter/IPython and non-jupyter/IPython contexts. If the library provides interactivity, an interactive figure is preferred.

After this kwarg, `show` can have any additional kwargs needed.

**NOTE**: When specifying options for `renderer`, `jpg` is preferred to `jpeg`.

#### What to return
`FIGURE.show()` should return `None`.

## `.save()` method
Every figure should have the knowledge of how to export itself to a file on disk.

> #### What currently exists
>  - matplotlib has `.savefig` which saves the current figure, with options `fname` and `format` among others.
> - plotly has `write_*` methods which write the figure to a `file` (or writable object) and return `None`.
> - bokeh has top level `export_*` methods for each output format as well as `save` (only outputs html) which both take a FIGURE as the arg.

### Proposal
Figures should have `FIGURE.save()` method for exporting.

There are several kwargs that `save` should include:

 1) `file`: should be a file object or optionally a path to a file. If the LIBRARY does not support file paths and a user provides one rather than a file object, the LIBRARY should raise a sensible error. Can default to some user configured value.
 2) `output`: should be a file format that the figure can be exported to i.e. `'png'`, `'jpg'`, `'svg'`, `'json'`, `'html'`. The library should set a default output and/or allow the user to configure the output. For instance matplotlib does:
    > If format is not set, then the output format is inferred from the extension of `fname`, if any, and from `rcParams["savefig.format"]` otherwise. If `format` is set, it determines the output format.
    >
    >from: [matplotlib.pyplot.savefig](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.savefig.html)

**NOTE**: These kwargs are deliberately selected to avoid builtins in python 3 which precludes `format`, but no longer precludes `file`.

After these two, `save` can have any additional kwargs needed.

**NOTE**: When specifying options for `output`, `jpg` is preferred to `jpeg`.

#### What to return
`FIGURE.save()` should return the file path or object that was saved to.

#### Some examples
Save to default location:

```python
FIGURE.save()
```

Save to a particular location:

```python
with open('/path/to/output.html', 'w') as file:
    FIGURE.save(file)
```

is equivalent to

```python
FIGURE.save(file='/path/to/output.html')
```

is equivalent to

```python
FIGURE.save('/path/to/output.html', output='html')
```
