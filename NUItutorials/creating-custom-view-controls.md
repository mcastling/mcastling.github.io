<a name="top"></a>
# Creating Custom View Controls 

In this tutorial:

[Overview](#overview)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>

<a name="overview"></a>
## Overview
NUI provides the ability to create custom view controls, in particular by extending the `CustomView' class.

<a name="customview"></a>
### The existing CustomView class
The NUI 'CustomView' class provides common functionality required by all views.

~~~{.cs}
    public class CustomView : ViewWrapper
~~~

OnInitialize : called after the Control has been initialized.
SetBackground : Set the background with a property map.
EnableGestureDetection : Allows deriving classes to enable any of the gesture detectors that are available
RegisterVisual : Register a visual by Property Index, linking an View to visual when required.
CreateTransition - Create a transition effect on the control - for animations.
RelayoutRequest : Request a relayout, which means performing a size negotiation on this view, its parent and children (and potentially whole scene)
OnStageConnection : Called after the view has been connected to the stage
 
There are several controls derived from `CustomView` objects in NUI, including:

* **Spin** control, which is used for continuously changing values when users can easily predict a set of values.

* **ContactView** which consists of four visuals (Image, Primitive, Text and Color).
  All of these visuals can be configured via properties - ImageURL (Image), Shape (Primitive), Name (Text) and Color.
  Tap gesture is also enabled on the ContactView which changes the color visual to some random color when the ContactView is tapped.

* A **VisualView** control enabling the addition of any visual. See [Visual View class](visuals.md#visualview)

[Back to top](#top)
 
<a name="guidelines"></a>
### General Guidelines:
+ Use **properties** as much as possible as Controls should be data driven.
  + These controls will be used through JavaScript and JSON files so need to be compatible.
+ The Control can be updated when the properties change (e.g. style change)
  + Ensure control deals with these property changes gracefully, on both first and subsequent times they are set
+ Use Visuals rather than creating several child Views.
  + DALi rendering pipeline more efficient
+ Accessibility actions should be considered when designing the Control.
+ Use `Events` if the application needs to be react to changes in the control state.
+ Use of `Gestures` should be preferred over analysing raw touch events.
 
[Back to top](#top)
 
<a name="rendering"></a>
### Rendering Content

To render content, the required views can be created and added to the control itself as its children.
However, this solution is not fully optimised and means extra views will be added, and thus, need to be processed in NUI.
 
It is recommended to use/reuse visuals [Visuals tutorial](visuals.md) to create the required content.

Visuals are usually defined in a stylesheet.
 
![ ](creating-custom-controls/rendering.png)

The following code snippet shows the creation and registration of an image visual in `ContactView.cs`.

~~~{.cs}

[ScriptableProperty()]
public string ImageURL
{
    get
        {
            return _imageURL;
        }
        set
        {
            _imageURL = value;

            // Create and Register Image Visual
            PropertyMap imageVisual = new PropertyMap();
            imageVisual.Add( Visual.Property.Type, new PropertyValue( (int)Visual.Type.Image ))
                       .Add( ImageVisualProperty.URL, new PropertyValue( _imageURL ) )
                       .Add( ImageVisualProperty.AlphaMaskURL, new PropertyValue( _maskURL ));
           _imageVisual =  VisualFactory.Get().CreateVisual( imageVisual );

            RegisterVisual( GetPropertyIndex("ImageURL"), _imageVisual );

            // Set the depth index for Image visual
           _imageVisual.DepthIndex = ImageVisualPropertyIndex;
        }
    }
~~~

`ScriptableProperty` is described [ScriptableProperty()](#xxxxxxxx)

`RegisterVisual` registers a visual by Property Index, linking a view to a visual when required.

The [Visuals tutorial](visuals.md) describes the property maps that can be used for each visual type.

[Back to top](#top)
 
<a name="stylable"></a>
### Ensuring Control is Stylable

The NUI property system allows custom controls to be easily styled. The JSON Syntax is used in the stylesheets:
 
**JSON Styling Syntax Example:**
~~~
{
  "styles":
  {
    "textfield":
    {
      "pointSize":18,
      "primaryCursorColor":[0.0,0.72,0.9,1.0],
      "secondaryCursorColor":[0.0,0.72,0.9,1.0],
      "cursorWidth":1,
      "selectionHighlightColor":[0.75,0.96,1.0,1.0],
      "grabHandleImage" : "{DALI_STYLE_IMAGE_DIR}cursor_handler_drop_center.png",
      "selectionHandleImageLeft" : {"filename":"{DALI_STYLE_IMAGE_DIR}selection_handle_drop_left.png" },
      "selectionHandleImageRight": {"filename":"{DALI_STYLE_IMAGE_DIR}selection_handle_drop_right.png" }
    }
  }
}
~~~
 
Styling gives the UI designer the ability to change the look and feel of the control without any code changes.
 
| Normal Style | Customized Style |
|:------------:|:----------------:|
|![ ](./Images/creating-custom-controls/popup-normal.png) | ![ ](./Images/creating-custom-controls/popup-styled.png)|
 
The [styling tutorial](styling.md) explains how to build up visuals for the button states using JSON stylesheets,
and transitioning between the various button states.

[Back to top](#top)
 
<a name="typeregistration"></a>
### Type Registration 

The TypeRegistry is used to register your custom control.

Type registration allows the creation of the control via a JSON file, as well as registering properties, signals and actions,
transitions and animation effects.
 
Type registration \endlink system which can be used to register
a derived actor/control type along with specifying a method which is used to create this type. This
type registration normally takes place at library load time.

Type Registration is via the `ViewRegistry` method, see [Custom View creation](#creation)

#### Properties
 
Control properties can be one of three types:
 + **Event-side only:** A function is called to set this property or to retrieve the value of this property.
                        Usually, the value is stored as a member parameter of the Impl class.
                        Other operations can also be done, as required, in this called function.
 + **Animatable Properties:** These are double-buffered properties that can be animated.
 + **Custom Properties:** These are dynamic properties that are created for every single instance of the control.
                          Therefore, these tend to take a lot of memory and are usually used by applications or other controls to dynamically set certain attributes on their children.
                          The index for these properties can also be different for every instance.
 
Careful consideration must be taken when choosing which property type to use for the properties of the custom control.
For example, an Animatable property type can be animated but requires a lot more resources (both in its execution and memory footprint) compared to an event-side only property.

~~~{.cs}
static ContactView()
{
    ViewRegistry.Instance.Register(CreateINstance, typeof(ContactView));
}
~~~

[Back to top](#top)
 
<a name="creation"></a>
### Custom View control Creation

It is recommended to do provide a New() method in the custom control implementation where the Initialize() method should be called.

~~~{.cs}
contactView = new ContactView()
~~~

The `ViewRegistry` registers the control type with the `typeRegistery`, and any scriptable properties via [ScriptableProperty](#xxxx )

Each custom C# view should have it's static constructor called before any JSON file is loaded.
Static constructors for a class will only run once ( they are run per control type, not per instance).
Inside the static constructor the control should register it's type with the ViewRegistry
e.g.
~~~{.cs}
static ContactView()
{
    ViewRegistry.Instance.Register(CreateINstance, typeof(ContactView));
}
~~~

The control should also provide a CreateInstance function, which gets passed to the ViewRegistry
Eventually it will be called if DALi Builder finds a Spin control in a JSON file
static CustomView CreateInstance()
{
    return new ContactView();
}

[Back to top](#top)
 
<a name=""></a>
### Enabling properties for JSON access

    internal class ScriptableProperty : System.Attribute

    /// Add this attribute to any property belonging to a View (control) you want to be scriptable from JSON

THe ScriptableProperty class is derived from System.Attribute and is a internal class that enables
a property to be registered
Scriptable attribute exists, then register it with the type registry.

The following code snippet is taken from the `ContactView` class:

~~~{.cs}
[ScriptableProperty()]
public int Shape
{
    get
    {
        return _shape;
    }
    set
    {
        _shape = value;

        ...
        ...
    }

~~~

### Control Behaviour

The `CustomViewBehaviour' enum specifies the following behaviour:-
 
| Behaviour                            | Description                                                                                                    |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------|
| ViewBehaviourDefault                 | Default behavior (size negotiation is on, style change is monitored, event callbacks are not called.           |
| DisableSIzeNegotiation               | If our control does not need size negotiation, i.e. control will be skipped by the size negotiation algorithm. |
| DisableStyleChangeSignals            | True if control should not monitor style change signals such as Theme/Font change.                             |
| RequiresKeyboardNavigationSupport    | True if need to support keyboard navigation.    
| LastViewBehaviour                    |                                                               |
___________________________________________________________________________________________________


### Touch, Hover & Wheel Events

+ A **touch** is when any touch occurs within the bounds of the custom view. Connect to the View class `TouchSignal` method.
+ A **hover event** is when a pointer moves within the bounds of a custom view (e.g. mouse pointer or hover pointer).
+ A **wheel event** is when the mouse wheel (or similar) is moved while hovering over a view (via a mouse pointer or hover pointer).
 
If the control needs to utilize hover and wheel events, then the correct behaviour flag should be used when constructing the control,
and then the appropriate method should be overridden.

___________________________________________________________________________________________________

### Gestures

DALi has a gesture system which analyses a stream of touch events and attempts to determine the intention of the user.
The following gesture detectors are provided:
 
 + **Pan:** When the user starts panning (or dragging) one or more fingers.
            The panning should start from within the bounds of the control.
 + **Pinch:** Detects when two touch points move towards or away from each other.
              The center point of the pinch should be within the bounds of the control.
 + **Tap:** When the user taps within the bounds of the control.
 + **LongPress:** When the user presses and holds on a certain point within the bounds of a control.
 
The control base class provides basic set up to detect these gestures.
If any of these detectors are required then this can be specified in the OnInitialize() method (or as required).
 
This code snippet is taken from the `ContactView` custom view control:

~~~{.cs}

        public override void OnInitialize()
        {
            // Enable Tap gesture on ContactView
            EnableGestureDetection(Gesture.GestureType.Tap);
        }
~~~

`EnableGestureDetection` Allows deriving classes to enable any of the gesture detectors that are available
The above snippet of code will only enable the default gesture detection for each type.
If customization of the gesture detection is required, then the gesture-detector can be retrieved and set up accordingly in the same method.
 
~~~{.cs}
PanGestureDetector panGestureDetector = GetPanGestureDetector();
panGestureDetector.AddDirection( PanGestureDetector.DIRECTION_VERTICAL );
~~~


 
Finally, the appropriate method should be overridden:

~~~{.cs}
OnPan( PanGesture& pan ) // Handle pan-gesture
~~~

~~~{.cs}
OnPinch( PinchGesture& pinch ) // Handle pinch-event
~~~

~~~{.cs}
OnTap( TapGesture& tap ) // Handle tap-gesture
~~~

~~~{.cs}
OnLongPress( LongPressGesture& longPress ) // Handle long-press-gesture
~~~

As an example here is the `OnTap` method from the `ContactView` class :

~~~{.cs}
        public override void OnTap(TapGesture tap)
        {
            // Change the Color visual of ContactView with some random color
            Random random = new Random();
            float nextRed   = (random.Next(0, 256) / 255.0f);
            float nextGreen = (random.Next(0, 256) / 255.0f);
            float nextBlue  = (random.Next(0, 256) / 255.0f);
            Animation anim = AnimateBackgroundColor( new Color( nextRed, nextGreen, nextBlue, 1.0f), 0, 2000 );
            anim.Play();
        }
~~~

### Accessibility

Accessibility is functionality that has been designed to aid usage by the visually impaired.
 
Accessibility behaviour can be customized in the control by overriding certain virtual methods. An example is `OnAccessibilityTouch`
Touch events are delivered differently in Accessibility mode. This method should be overridden if some special behaviour
is required when these touch events are received. 

### Stage Connection

Methods are provided in `CustomView' class that can be overridden if notification is required when our control
is connected to or disconnected from the stage. (default window)

~~~{.cs}
OnStageConnection( int depth )
~~~

and 

~~~{.cs}
OnStageDisconnection()
~~~
 
### Size Negotiation

The RelayoutController is an object that is private in DALi Core. It's main job is to take relayout requests from actors.
It can be enabled or disabled internally. If disabled, then all relayout requests are ignored. By default the relayout controller is disabled until just after the
initial application initialize. This allows the scene for an application to be created without generating many relayout requests. After the application
has initialized the scene, then the relayout controller is automatically enabled and a relayout request is called on the root of the scene. This request spreads down the scene
hierarchy and requests relayout on all actors that have size negotiation enabled.

Relayout requests are put in automatically when a property is changed on an actor or a change to the stage hierarchy is made and manual requests are usually not necessary.


In addition to the **resize policies** detailed in the Size Negotiation Programming Guide there is one additional policy available to control writers:

<h3>Initialization</h3>
Size negotiation is enabled on controls by default. If a control is desired to not have size negotiation enabled then simply pass in the
DISABLE_SIZE_NEGOTIATION flag into the Control constructor.

Override the `OnRelayout` method to position and resize the buttons. 

Size Negotiation API

<h3>Base Class Methods</h3>

The base class methods are used to call functionality held in Actor and are defined in CustomActorImpl.

There is a RelayoutRequest method defined. This method is available for deriving controls to call when they would like themselves to be relaid out.
@code void RelayoutRequest() @endcode


<h3>Overridable Methods</h3>
These overridable methods in control provide customization points for the size negotiation algorithm.

<h4>Responding to the Change of Size on a Control</h4>

OnRelayout is called during the relayout process at the end of the frame immediately after the new size has been set on the actor. If the actor has calculated
the size of child actors then add them to container with their desired size and set the ResizePolicy::USE_ASSIGNED_SIZE resize policy on them.
At this point the size of the actor has been calculated so it is a good place to calculate positions of child actors etc.
@code virtual void OnRelayout( const Vector2& size, RelayoutContainer& container ) @endcode

The OnRelayoutSignal signal is raised after SetSize and OnRelayout have been called during the relayout processing at the end of the frame. If the control is deriving
from Control then the OnRelayout virtual is preferred over this signal. The signal is provided for instance when custom code needs to be run on the
children of an actor that is not a control.

Events

OnSetResizePolicy is called when the resize policy is set on an actor. Allows deriving actors to respond to changes in resize policy.
virtual void OnSetResizePolicy( ResizePolicy::Type policy, Dimension::Type dimension ) 

The following methods must be overridden for size negotiation to work correctly with a custom control.
 
`GetNaturalSize` : Return the natural size of the view
 
`GetHeightForWidth( float width )` : Called by the size negotiation algorithm if we have a fixed width
`GetWidthForHeight( float height ) : Called by the size negotiation algorithm if we have a fixed height
 OnRelayout( const Vector2& size, RelayoutContainer& container ) : Called after the size negotiation has been finished for this control.<br>
        /// The control is expected to assign this given size to itself/its children.<br>
        /// Should be overridden by derived classes if they need to layout views differently after certain operations like add or remove views, resize or after changing 
~~~

[Back to top](#top)

