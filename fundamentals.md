
# DALi Fundamentals

## Scene Graph

From Wikipedia...
  
A scene graph is a collection of nodes in a graph or tree structure.
A node may have many children but often only a single parent,
with the effect of a parent applied to all its child nodes;
an operation performed on a group automatically propagates
its effect to all of its members. In many programs, associating
a geometrical transformation matrix (see also transformation and matrix)
at each group level and concatenating such matrices together is an
efficient and natural way to process such operations. A common feature,
for instance, is the ability to group related shapes/objects into a
compound object that can then be moved, transformed, selected,
etc. as easily as a single object.

### Objects and nodes in DALi
The DALi scene graph consists of objects (nodes) such as images, text and buttons.

Views are also effectively nodes which receive input (touch events), and act as a
container for draw-able elements (which are also nodes), and other views.

## Views and the Stage

A _View_ is the primary object with which DALi applications interact. Views can be visible
(UI component) or invisible (a camera view).

A DALi application uses a hierarchy of View objects to position visible content.
A View inherits a position relative to its parent, and can be moved relative to this point.
UI controls can be built by combining multiple Views.

The _Stage_ is a top-level object used for displaying a tree of views.

A View must be added to the stage to display its contents.

Note: The term **'View'** has now replaced the term **'Actor'**.

<hr>

### Software example
The following example shows how to connect a new view to the stage:

~~~{.cs}
View view = new View();
Stage.GetCurrent().Add(view);
~~~
