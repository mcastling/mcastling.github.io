# DALi Fundamentals

## The Coordinate System

The Stage has a 2D size, which matches the size of the application window.
The default **unit 1 is 1 pixel with default camera and** the default coordinate system in
DALi has the **origin at the top-left corner, with positive X to right, and position Y going
downwards**.  This is intended to be convenient when laying-out 2D views.

![ ](./Images/coordinate-system-and-stage.png)

## Positioning Views

A view inherits its parent's position. The relative position between the view and parent is determined by 3 properties:

1) ParentOrigin.  This Vector3 property defines a point within the parent views's area.

![ ](./Images/parent-origin.png)

The default is "top-left", which can be visualized in 2D as (0, 0), but is actually Vector3(0, 0, 0.5) in the 3D DALi world.  The views's position is relative to this point.

2) AnchorPoint.  This Vector3 property defines a point within the child views's area.

![ ](./Images/anchor-point.png)

The default is "center", which can be visualized in 2D as (0.5, 0.5), but is actually Vector3(0.5, 0.5, 0.5) in the 3D DALi world.  The views's position is also relative to this point.

3) Position.  This is the position vector between the parent-origin and anchor-point.

![ ](./Images/actor-position.png)

The default is (X = 0, Y = 0), so a view would appear centred around the top left corner of its parent, if it is 
placed directly without modifying the parent origin, anchor point or position.

A view added directly to the stage with position (X = stageWidth*0.5, Y = stageHeight*0.5), would appear in the center of the screen.  Likewise a view with position (X = viewWidth*0.5, Y = viewWidth*0.5), would appear at the top-left of the screen. However, basic positioning is normally done via changing the parent origin and/or anchor point instead - use ParentOrigin::CENTER and AnchorPoint::CENTER to place the view in the center of the screen, and ParentOrigin::TOP_LEFT and AnchorPoint::TOP_LEFT to place it inside the screen on the top left. Note that since DALi is a 3D toolkit, this behaviour is the result of a default perspective camera setup.


