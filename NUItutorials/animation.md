<a name="top"></a>
# Animation tutorial

This tutorial describes the NUI animation framework.

This tutorial covers the following subjects:

[Animation Overview](#overview)<br>
[Animation creation](#creation)<br>
[Animating properties](#animatingproperties)<br>
[Animation methods](#animationmethods)<br>
[](#)<br>
[](#)<br>
[](#)<br>
[](#)<br>

[Animation nultithreading](#)<br>
[working example](#)<br>

The tutorial includes a working example.

<a name="overview"></a>
## Animation Overview

Enables you to create animated effects.

NUI provides a rich and easy to use animation framework which allows the creation of visually rich applications.

NUI animations occur in a dedicated thread. This allows animations to run smoothly, regardless of the time
taken to process the input, events, and other factors in the application code. See [Animation multithreading](#multithreading).

![](NUI_Class_Hierarchy.png) shows the Animation classes in the NUI class hierarchy. The `Animatable` clss contains 'property' 
methods such as `GetProperty` and `IsPropertyAnimatable`. The `Animation` class contains the animation methods such as `AnimateBy`.
The `Animation` class can be used to animate the animatable properties of any number of objects, typically View's.

[Back to top](#top)

<a name="creation"></a>
### Creating a basic Animation

Here an animation object is created, and the duration of the animation will be 2 seconds:

~~~{.cs}
_animation = new Animation( 2000 );
~~~

alternatively,

~~~{.cs}
_animation = new Animation
{
    Duration = 2000;
};
~~~

[Back to top](#top)

<a name="animatingproperties"></a>
### Animating Properties

There are two distinct ways in which properties can be animated within DALi:
- **AnimateTo:** The property will animate **TO** the value in the given time.
- **AnimateBy:** The property will animate **BY** the value in the given time.

Here is an example:

(Assume view1 and view2 are at position 10.0f, 10.0f, 0.0f at the start of the animation).
~~~{.cs}
// Animate the position of view1 TO 10.0f, 50.0f, 0.0f
animation.AnimateTo( view1, View.Property.POSITION, Vector3(10.0f, 50.0f, 0.0f) ); 		// End Position: 10.0f, 50.0f, 0.0f

// Animate the position of view2 BY 10.0f, 50.0f, 0.0f
animation.AnimateBy( view2, View.Property.POSITION, Vector3(10.0f, 50.0f, 0.0f) ); 	// End Position: 20.0f, 60.0f, 0.0f
~~~

Another example taken from the working sample in this tutorial....
~~~{.cs}
_animation.AnimateTo(_text, "Orientation", new Rotation(new Radian(new Degree(180.0f)), PositionAxis.X), 0, 500);
_animation.AnimateTo(_text, "Orientation", new Rotation(new Radian(new Degree(0.0f)), PositionAxis.X), 500, 1000);
_animation.AnimateBy(_text, "ScaleX", 3, 1000, 1500);
_animation.AnimateBy(_text, "ScaleY", 4.0f, 1500, 2000);

See [Animation methods](#animationmethods) for explanation of parameters.

~~~

[Back to top](#top)

<a name="animationmethods"></a>
### Animation methods

The `Animation` class provides a series of overloaded methods for animation of properties.

* **AnimateBy** animates a property value by a relative amount.

~~~{.cs}
public void AnimateBy(View target, string property, object relativeValue, AlphaFunction alphaFunction = null)
~~~

_target_        	The target object to animate<
_property_      	The target property to animate, can be enum or string
_relativeValue_ 	The property value will change by this amount
_alphaFunction_ 	The alpha function to apply

* **AnimateTo** animates a property to a destination value.

~~~{.cs}
public void AnimateTo(View target, string property, object destinationValue, AlphaFunction alphaFunction = null)
~~~

_destinationValue_      The destination value

~~~{.cs}
public void AnimateTo(View target, string property, object destinationValue, int startTime, int endTime, AlphaFunction alphaFunction = null)
~~~

_startTime_		Start time of animation
_endTime_		End time of animation

* **AnimateBetween** animates a property between [keyframes](#zzz).

~~~{.cs}
public void AnimateBetween(View target, string property, KeyFrames keyFrames, Interpolation interpolation = Interpolation.Linear, AlphaFunction alphaFunction = 
~~~

_keyFrames_		The set of time/value pairs between which to animate
_interpolation_		The method used to interpolate between values

* **AnimatePath** animates a view's position and orientation through a predefined path.

~~~{.cs}
public void AnimatePath(View view, Path path, Vector3 forward, AlphaFunction alphaFunction = null)
~~~

_path_			Defines position and orientation
_forward_ 		The vector (in local space coordinate system) that will be oriented with the path's tangent direction</param>

### Playback Control

When an animation is created, it can be played:
~~~{.cs}
animation.Play();
~~~

Stop and Pause are also supported.
~~~{.cs}
animation.Stop();
animation.Pause();
~~~

To loop the animation to play multiple times:

~~~{.cs}
animation.SetLooping(true);
~~~

By default, when an animation ends, the properties that it was animating are BAKED.
However, the property changes can be **discarded** when the animation ends (or is stopped):

~~~{.cs}
animation.EndAction = Animations.EndActions.Discard
~~~

### Events

Applications can be notified when the animation finishes:

~~~{.cs}
_animation.Finished += AnimationFinished;
~~~

~~~{.cs}
public void AnimationFinished(object sender, EventArgs e)
{
    Tizen.Log.Debug("NUI", "AnimationFinished()");

    ...
    ...

}


Applications can be notified when the animation has reached a given progress percentage

~~~{.cs}
_animation.ProgressReached += progressReached;
~~~

        /// <summary>
        /// Gets/Sets the Progress notification marker which triggers the ProgressReachedSignal.<br>
        /// percentage of animation progress should be greater than 0 and less than 1, e.g 0.3 for 30% <br>
        /// One notification can be set on each animation
        /// </summary>
        public float ProgressNotification
 ~~~{.cs}
_animation.ProgressNotification = 0.5; // trigger progress reached signal at 50% of animation time
~~~

### Alpha Functions

Alpha functions are used in animations to specify the rate of change of the animation parameter over time. This allows
the animation to be accelerated, decelerated, repeated or bounced. The built in supported functions can be viewed
in the `AlphaFunction` class.

  public enum BuiltinFunction {
    Default,
    Linear,
    Reverse,
    EaseInSquare,
    EaseOutSquare,
    EaseIn,
    EaseOut,
    EaseInOut,
    EaseInSine,
    EaseOutSine,
    EaseInOutSine,
    Bounce,
    Sin,
    EaseOutBack,
    Count
  }

It is possible to specify a different alpha function for each animator in an Animation object:

~~~{.cs}
animation.AnimateTo(view1, View.Property.Position, Vector3(10.0f, 50.0f, 0.0f), new AlphaFunction.BuiltinFunctions.Linear);
~~~





You can create your own alpha function:

~~~{.cs}

AlphaFunction af(alphafunc);
animation.SetDefaultAlphaFunction(af);

~~~

[Back to top](#top)

### Animation Types

NUI supports key frame and path animation.

#### Key-Frame Animation

When you play the animation, DALi will animate the position of actor1 between the key-frames specified.
'actor1' will animate from (10.0f, 10.0f, 10.0f) to (200.0f, 200.0f, 200.0f) by 70% of the animation time,
and then spend the remaining time animating back to (100.0f, 100.0f, 100.0f).

The advantage of specifying a key-frame at 0% is that regardless of where 'actor1' is, it will start from position (10.0f, 10.0f, 10.0f).
If `AnimateTo` was used, then the start position would have been actor1's current position.

DALi provides support for animating between several different values, i.e. key-frames.
A key frame takes a progress value between 0.0f and 1.0f (0 and 100% respectively) and portrays the value of the
property when the animation has progressed that much.

You can create several key frames:

~~~{.cs}
KeyFrames keyFrames = new KeyFrames();
keyFrames.Add( 0.0f, Vector3( 10.0f, 10.0f, 10.0f ) );
keyFrames.Add( 0.7f, Vector3( 200.0f, 200.0f, 200.0f ) );
keyFrames.Add( 1.0f, Vector3( 100.0f, 100.0f, 100.0f ) );
~~~

And then add them to your animation:

~~~{.cs}
animation.AnimateBetween( view1, View.Property.Position), keyFrames );
~~~

Here is a more comprehensive sample of 'Key frame' use:

~~~{.cs}

// from FocusEffect.cs
focusData.ImageItem.Size = new Size(100.0f, 100.0f, 0.0f);
parentItem.Add(focusData.ImageItem);

Size targetSize = focusData.TargetSize;
Size initSize = focusData.InitSize;

...
...
...

KeyFrames keyFrames = new KeyFrames();

keyFrames.Add(0.0f, initSize);
keyFrames.Add(focusData.KeyFrameStart, initSize);
keyFrames.Add(focusData.KeyFrameEnd, targetSize);

// for halo add an extra keyframe to shrink it ( in 20% of time after it has finished)
if (focusData.Name == "halo")
{
   keyFrames.Add(focusData.KeyFrameEnd + 0.2f, initSize);
}

_animation.AnimateBetween(focusData.ImageItem, "Size", keyFrames, Animation.Interpolation.Linear, new AlphaFunction(AlphaFunction.BuiltinFunctions.EaseOutSine));

~~~


## Path Animations

A `Path` can be used to animate the position and orientation of actors.

![ ](animation/animated-path.png)

The black points in the diagram are points where the logo will travel to. The red points are the control points which
express the curvature of the path on the black points.

This, in code will be represented as follows:
~~~{.cpp}
Path path = Path::New();
path.AddPoint( Vector3( 50.0f, 10.0f, 0.0f ));
path.AddPoint( Vector3( 90.0f, 50.0f, 0.0f ));
path.AddPoint( Vector3( 10.0f, 90.0f, 0.0f ));
~~~
The control points can be added manually using Dali::Path::AddControlPoint, or Path can auto-generate them for you:
~~~{.cs}
path.GenerateControlPoints(0.25f);
~~~
Here 0.25f represents the curvature of the path you require. Please see Dali::Path::GenerateControlPoints for more information.

To animate actor1 along this path:
~~~{.cpp}
animation.Animate( actor1, path, Vector3::ZERO );
~~~
The third parameter is the forward vector (in local space coordinate system) that will be oriented with the path's tangent direction.


// from test1.cs
            Position position0 = new Position(200.0f, 200.0f, 0.0f);
            Position position1 = new Position(300.0f, 300.0f, 0.0f);
            Position position2 = new Position(400.0f, 400.0f, 0.0f);

            Path path = new Path();
            path.AddPoint(position0);
            path.AddPoint(position1);
            path.AddPoint(position2);

            //Control points for first segment
            path.AddControlPoint(new Vector3(39.0f, 90.0f, 0.0f));
            path.AddControlPoint(new Vector3(56.0f, 119.0f, 0.0f));

            //Control points for second segment
            path.AddControlPoint(new Vector3(78.0f, 120.0f, 0.0f));
            path.AddControlPoint(new Vector3(93.0f, 104.0f, 0.0f));

            Animation animation = new Animation();
            animation.AnimatePath(view, path, Vector3.XAxis, 0, 5000, new AlphaFunction(AlphaFunction.BuiltinFunctions.Linear));
            animation.Play();

[Back to top](#top)

<a name="multithrerading"></a>
### Multi-threaded Architecture

NUI animations and rendering occur in a dedicated rendering thread. This allows animations to run smoothly, regardless of the time
taken to process inputs events etc. in application code.

Internally NUI contains a scene-graph, which mirrors the Views hierarchy. The scene-graph objects perform the actual animation & rendering,
whilst Views provide thread-safe access.

An example view hierarchy is shown below, in which one of the views is being animated. The objects in green are created by the
application code, whilst the private objects in blue are used in the dedicated rendering thread.

![](multi-threaded-animation.png)

#### Reading an animated value

When a property is animatable, it can only be modified in the rendering thread. The value returned from a getter method, is the value used when the
previous frame was rendered.

For example `pos = view.Position` returns the position at which the View was last rendered. Since setting position `view.Position = pos`
is asynchronous, `pos = view.Position` won't immediately return the same value.

~~~{.cs}
// Whilst handling an event...

View view = new View();
Window.Instance.Add(view); // initial position is 0,0,0
view.Position = new Vector3(10,10,10);

Vector3 current = view.Position;
Console.WriteLine("Current position: " + current.X + ", " + current.Y + ", " + current.Z);
Console.WriteLine("...");

// Whilst handling another event...

current = view.Position;
Console.WriteLine("Current position: " + current.X + ", " + current.Y + ", " + current.Z);

~~~

The example code above would likely output:

"Current position: 0,0,0"
"..."
"Current position: 10,10,10"

#### Setting a property during an animation

When a property is being animated, the Animation will override any values set e.g. `current = view.Position;`

![](multi-threaded-animation-2.png)

The order of execution in the render thread is:

~~~{.cs}
1. Process message => SetPosition
2. Apply animation => SetPosition
3. Render frame
~~~

<a name="workingexample"></a>
### Simple Animation example

A simple animation 'hello world' example has been created to help in illustration some of the principles outlined in this guide: 

    1. Download [Animation example source code](http://dalihub.github.io/NUIsetup/animation-hello-world.cs)
    2. Copy this file to your nuirun/src/public folder

To build this example, follow the Build and Run section of [Ubuntu setup guide](setup-ubuntu.md#buildnui)

[Back to top](#top)

### Animation class Properties

`Animation` class properties include:

| Property     | Type         | Description
| ------------ | ------------ | ------------                       |
|              |              |                                    |
|              |              |                                    |
|              |              |                                    |
|              |              |                                    |
|                             |              |                                    |
|                             |              |                                    |
|                              |              |                                    |
|                               |              |                                    |
| SpeedFactor                      |     float         | Gets/Sets Specifies a speed factor for the animation.                                   |
| ProgressNotification         |    float          |    Gets/Sets the Progress notification marker which triggers the ProgressReachedSigna                                |



