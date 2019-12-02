- Spec: 3
- Title: Methods on children of figures
- Version: 0.1
- Last-Modified: 2019-07-19
- Authors: [Jon Mease](jon.mease@gmail.com),
           [Julia Signell](jsignell@gmail.com),
- Status: Active
- Type: Standards
- Content-Type: text/markdown
- Created: 2019-07-19

## Abstract
This spec describes a proposed shared API for accessing objects within the figure hierarchy from various plotting libraries.

This spec uses **figure** to refer to any object that has a visible representation but which can also be saved, exported, etc. This includes generic visualizations or animations that may or may not be considered a "plot" with axes, ticks, and so on.

When referring to parts of a figure (such as axes and ticks) we'll use the term: **child**.

When detailing a method, this spec uses FIGURE to refer to any specific figure object and LIBRARY to refer to any specific visualization library, and CHILD to refer to the child of a figure.

## If a method doesn't make sense
If a given operation doesn't make sense for that library, then it can satisfy the spec by simply having that method return a message to that effect ("Unsupported: LIBRARY does not provide JSON output").

## Accessing root figure
Every child should know what figure it belongs to.

### Proposal
Every object in the figure hierarchy should have a `.root` property that returns the top-level parent figure. A deeply nested child object would not return its immediate parent, but the highest-level parent it is aware of (which may be its parent, grandparent, great-grandparent, etc., depending on how deeply it is nested). If the object doesn't belong to a figure then it should return `None`.

> #### What currently exists
> plotly calls these "graph objects" and returns them on the `figure` property.
