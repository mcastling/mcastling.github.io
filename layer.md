# Layer

 Layers provide a mechanism for overlaying groups of views on top of each other.
 Layers can also clip their contents to exclude any content outside a user defined area.
  
 ![ ](layers.png)
  
 When a layer is added to the stage it is assigned a unique depth value. By default the stage has a root layer with a depth value of 0.
  
 Layers are views and they inherit position, orientation and scale of their parent view.
 Layers are drawn in an order determined by a layer depth value.
 The depth buffer is cleared before each layer is rendered, unless depth
 test is disabled or there's no need for it based on the layers contents.

**Note: Layers work independently of the view hierarchy.**
Layers can be positioned anywhere in the view tree. The layer draw order is always defined by the getDepth() value.
  
~~~{.cs}
// C# example of adding a view to the root layer

//  using stage.add() will automatically add view to the root layer
Stage stage = Stage.GetCurrent();
stage.add( myView );

// Or you can explicitly add a view to the root layer.
Layer rootLayer = stage.GetRootLayer();
rootLayer.add( myView );  // adds a view to the root layer

// rootLayer.getDepth() == 0

~~~

Example To create two new layers on top of the root layer.
  

~~~{.js}
// JavaScript 

var layer1 = new dali.Layer();
var layer2 = new dali.Layer();

// the initially depth order of each layer, depends on the order
// it is added to the stage.

dali.stage.add( layer1 );  // will be drawn on top of root layer
dali.stage.add( layer2 );  // will be drawn on top of layer 1

dali.stage.add( myView1);
layer1.add( myView2);
layer2.add( myView3);

// dali.stage.getRootLayer().getDepth = 0  myView1 drawn first ( on bottom )
// layer1.getDepth() == 1                  myView2 drawn second ( in middle )
// layer2.getDepth() == 2                  myView3 drawn last ( on top )
~~~

### Layer clipping

Clips the contents of the layer to a rectangle.

~~~{.js}
// JavaScript

layer1.anchorPoint = dali.CENTER;
layer1.parentOrigin = dali.CENTER;
layer1.clippingEnable = true;
layer1.clippingBox = [20,20,100,100];  // X, Y, Width, Height
~~~

~~~{.cpp}
// C++

layer1.SetAnchorPoint( AnchorPoint::CENTER );
layer1.SetParentOrigin( ParentOrigin::CENTER );
layer1.SetClipping( true );
layer1.SetClippingBox( 20, 20, 100, 100 ); // X, Y, Width, Height

~~~

### Re-ordering layers

The following functions can be used to change the draw order of the layers.


 - Raise() Raise the layer up by 1
 - Lower() Lower the layer down by 1
 - RaiseAbove( layer ) Ensures the layers depth is greater than the target layer
 - LowerBelow( layer ) Ensures the layers depth is less than the target layer
 - RaiseToTop() raise the layer to the top
 - LowerToBottom() lower the layer to the bottom
 - MoveAbove( layer ) Moves the layer directly above the given layer.
 - MoveBelow( layer ) Moves the layer directly below the given layer.

Note:
 - The root layer can be moved above and below other layers. However, stage.add( view ), will always use the root layer object.

### Rendering order of views inside of a layer

Layers have two behaviour modes:

 - LAYER_UI ( Default )
 - LAYER_3D

### Layer_UI

~~~{.js}
// JavaScript
layer.behaviour = "LAYER_2D";
~~~

~~~{.cs}
// C#
layer.SetBehavior( Layer::LAYER_2D );
~~~

#### Background

 - Graphics are drawn in DALi using renderers
 - Views can have zero or many renderers
 - Renderers can be shared by views
 - Renderers have a depth index property
 - In LAYER_2D mode, draw order of a renderer within a layer = Tree depth + renderer depth index
  
 When using  Layer_2D mode depth testing is disabled (depth buffer not used).
  
  With LAYER_2D, the draw order of the renderers is defined by both:

 - Renderer depth index.
 - Position of view in the view tree
  

Example:
  
We have two layers below. Everything in the root layer is drawn first.
If we did dali.stage.getRootLayer().raiseToTop(), then the root layer would be drawn last.

  
![ ](layer2d.png)
  

The formula for calculating the draw order of a renderer is:  depthIndex + ( TREE_DEPTH_MULTIPLIER * tree depth ).
Currently Layer::TREE_DEPTH_MULTIPLIER == 1000:
~~~
 Root (root layer) ( depth index offset of  0)
 +->  View1    ( depth index offset of 1000)
 ++-> View2    ( depth Index offset of 2000)
 +++-> View3   ( depth Index offset of 3000)
 +++-> View4   ( depth Index offset of 3000)
 +++-> View5   ( depth Index offset of 3000)
 +++-> Layer1   ( depth Index has no meaning for layers, layer draw order is independent of the hierarchy).
 ++++-> View6   ( depth Index offset of 4000)
 ++++-> View7   ( depth Index offset of 4000)
 ++++-> View8   ( depth Index offset of 4000)
~~~
  
Renderers with higher depth indices are rendered in front of renderers with smaller values.
  
Everything in the root layer gets rendered first, views 1..5
Then layer 1, viewss 6..8
  
If we want to determine draw order of views 6..8, we set the depthIndex on their renderers.
For example if we want the render draw order to be 8, 7, 6, with 6 being drawn last.

~~~{.js}
  var rendererForView6 = new dali.Renderer( geometry, material );
  var rendererForView7 = new dali.Renderer( geometry, material );
  var rendererForView8 = new dali.Renderer( geometry, material );

  rendererForView6.depthIndex = 2;   // drawn on top ( last)
  rendererForView7.depthIndex = 1;   // draw in the middle
  rendererForView8.depthIndex = 0;   // drawn on bottom ( first)

  daliview6.addRenderer( rendererForView6 );  // renderer 6 drawn with index of 2 + 4000 = 4002
  daliview7.addRenderer( rendererForView7 );  // renderer 7 drawn with index of 1 + 4000 = 4001
  daliview8.addRenderer( rendererForView8 );  // renderer 8 drawn with depth index of 0 + 4000 = 4000

~~~

### Layer_3D

~~~{.js}
// JavaScript
layer.behaviour = "LAYER_3D";
~~~

~~~{.cs}
// C#
layer.SetBehavior( Layer::LAYER_3D );
~~~
  
When using this mode depth testing will be used ( depth buffer enabled ).
  
Opaque renderers are drawn first and write to the depth buffer.
  
Then transparent renderers are drawn with depth test enabled but depth write switched off.
  
 ![ ](layers3d.png)

  
Transparent renderers are drawn in order of distance
from the camera ( painter's algorithm ).

 ![ ](transSort.png)
  

Note:

 - In LAYER_3D mode, view tree hierarchy makes no difference to draw order
 - When 2 transparent renderers are the same distance from the camera, you can use depth index to adjust which renderer is drawn first.

  
### View drawMode OVERLAY_2D

Inside a layer it is possible to force the tree views to be drawn on top of everything else in the layer.
  
The draw order of the views inside the tree marked OVERLAY_2D, is the draw order as defined by the renderers depth index.
Depth testing is not used.
  

Example:

~~~{.js}
// JavaScript
var layer = new dali.Layer();

layer.behaviour = "LAYER_3D"

dali.stage.add( layer );

layer.add( myView1 );
layer.add( myView2 );

myView3.drawMode = "OVERLAY_2D";

layer.add( myView3 );   // view 3 is drawn on top of view 1 and 2 as it's in the OVERLAY.

myView3.add( myView4 ); // view 4 is drawn on top of view 3, which is drawn on top of view 1 and 2.

myView3.add( myView5);  // the depth index of view 4 and 5 renderers will determine which is drawn first
~~~

  



### Layer View Specific Properties

| Name                   |    Type    | Writable     | Animatable|
|------------------------|------------|--------------|-----------|
| clippingEnable         |BOOLEAN     | 0     |  X |
| clippingBox            | ARRAY [0,0,400,600]) | 0 | X|
| behaviour              | STRING ( "LAYER_2D" or "LAYER_3D") | 0 | X|

