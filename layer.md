# Layers

A layer acts like a transparent sheet upon which shapes can be placed, sub-layers (layers within layers to any desired depth
are supported. Layers provide a mechanism for overlaying groups of views on top of each other.
Layers can also clip their contents to exclude any content outside a user defined area.

Layers can be 2D or 3D, defined by their _behaviour_ property.
  
 ![ ](layers.png)
  
When a layer is added to the stage it is assigned a unique depth value. By default the stage has a root layer
with a depth value of 0.
  
Layers are Views inherit the position, orientation and scale of their parent View.
Layers are drawn in an order determined by a layer depth value.
 
**Note: Layers work independently of the View hierarchy.**
Layers can be positioned anywhere in the View tree. The layer draw order is always defined by the _depth_ value.

~~~{.cs}
// C# example of adding a view to the root layer

//  using stage.add() will automatically add view to the root layer
Stage stage = Stage.GetCurrent();
stage.add( myView );

// Or you can explicitly add a view to the root layer.
Layer rootLayer = stage.GetRootLayer();
rootLayer.add( myView );  // adds a view to the root layer

~~~  

### Layer View Specific Properties

 - clippingEnable
 - clippingBox
 - behaviour can be "LAYER_UI" which is default, or "LAYER_3D"
  
### Layer clipping

Clips the contents of the layer to a rectangle.

### Re-ordering layers

A range of functions are provided to change the draw order of the layers.

### Rendering order of Views inside of a layer

#### Background

 - Graphics are drawn in DALi using renderers
 - Views can have zero or many renderers
 - Renderers can be shared by Views
 - Renderers have a depth index property
  
  With LAYER_UI, the draw order of the renderers is defined by both:

 - Renderer depth index.
 - Position of view in the view tree

  
![ ](layer2d.png)
  

### Layer_3D

Set Layer behaviour to "LAYER_3D";
  
Opaque renderers are drawn first and writen to the depth buffer.
 
 ![ ](layers3d.png)

  
Transparent renderers are drawn in order of distance from the camera ( painter's algorithm ).

 ![ ](transSort.png)

  
### View drawMode OVERLAY_2D

Inside a layer it is possible to force a tree views to be drawn on top of everything else in the layer.



