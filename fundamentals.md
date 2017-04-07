
# DALi Fundamentals

## Views and the Stage

A _View_ is the primary object with which DALi applications interact in C#.
A DALi application uses a hierarchy of view objects to position visible content.
A view inherits a position relative to its parent, and can be moved relative to this point.
UI controls can be built by combining multiple views.

The _Stage_ is a top-level object used for displaying a tree of views.

A view must be added to the stage to display its contents.

The following example shows how to connect a new view to the stage:

~~~{.cs}
View view = new View();
Stage.GetCurrent().Add(view);
~~~

~~~{.js}
var actor = new dali.Actor();
dali.stage.add( actor );
~~~

## The Coordinate System

The Stage has a 2D size, which matches the size of the application window.
The default **unit 1 is 1 pixel with default camera and** the default coordinate system in DALi has the **origin at the top-left corner, with positive X to right, and position Y going
downwards**.  This is intended to be convenient when laying-out 2D views.

![ ](coordinate-system-and-stage.png)


## Positioning Views

A view inherits its parent's position.  The relative position between the view and parent is determined by 3 properties:

1) ParentOrigin.  This Vector3 property defines a point within the parent views's area.

![ ](parent-origin.png)

The default is "top-left", which can be visualized in 2D as (0, 0), but is actually Vector3(0, 0, 0.5) in the 3D DALi world.  The views's position is relative to this point.

2) AnchorPoint.  This Vector3 property defines a point within the child views's area.

![ ](anchor-point.png)

The default is "center", which can be visualized in 2D as (0.5, 0.5), but is actually Vector3(0.5, 0.5, 0.5) in the 3D DALi world.  The views's position is also relative to this point.

3) Position.  This is the position vector between the parent-origin and anchor-point.

![ ](actor-position.png)

The default is (X = 0, Y = 0), so a view would appear centred around the top left corner of its parent, if it is 
placed directly without modifying the parent origin, anchor point or position.

A view added directly to the stage with position (X = stageWidth*0.5, Y = stageHeight*0.5), would appear in the center of the screen.  Likewise a view with position (X = viewWidth*0.5, Y = viewWidth*0.5), would appear at the top-left of the screen. However, basic positioning is normally done via changing the parent origin and/or anchor point instead - use ParentOrigin::CENTER and AnchorPoint::CENTER to place the actor in the center of the screen, and ParentOrigin::TOP_LEFT and AnchorPoint::TOP_LEFT to place it inside the screen on the top left.

Note that since DALi is a 3D toolkit, this behaviour is the result of a default perspective camera setup.

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

### How does this relate to the DALi public API?

Views are effectively nodes that receive input (touch events) and act as a
container for draw-able elements (which are also nodes), and other views.
