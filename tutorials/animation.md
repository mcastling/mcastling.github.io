<!--
/**-->

# Animation Tutorial {#animation}

This tutorial provides an introduction to the DALi Animation framework.

Animations enable objects to move and change their properties for a specified duration.

DALi provides a rich and easy to use animation framework.

Dali::Animation can be used to animate the properties of objects such as Views

DALi animations occur in a dedicated thread, allowing smooth animations.

## Creating a basic Animation {#animation-basics}

The __Dali::Animation__ class enables animation of the animatable **properties** of objects.

Create an animation object that will take place over 3 seconds:
~~~{.cpp}
Dali::Animation animation = Animation::New( 3.0f );
~~~

### Animating Properties

There are two distinct ways in which properties can be animated within DALi:
- **AnimateTo:** The property will animate **TO** the value in the given time.
- **AnimateBy:** The property will animate **BY** the value in the given time.

(Assume actor1 & actor2 are at position 10.0f, 10.0f, 0.0f at the start of the animation)
~~~{.cpp}
// Animate the position of actor1 TO 10.0f, 50.0f, 0.0f
animation.AnimateTo( Property( actor1, Dali::Actor::Property::POSITION ), Vector3(10.0f, 50.0f, 0.0f) ); // End Position: 10.0f, 50.0f, 0.0f

// Animate the position of actor2 BY 10.0f, 50.0f, 0.0f
animation.AnimateBy( Property( actor2, Dali::Actor::Property::POSITION ), Vector3(10.0f, 50.0f, 0.0f) ); // End Position: 20.0f, 60.0f, 0.0f
~~~

### Playback Control

When an animation is created, it can be played:
~~~{.cpp}
animation.Play();
~~~

Stop and Pause are also supported.
~~~{.cpp}
animation.Stop();
animation.Pause();
~~~

### Animatable properties

If a property is animatable

~~~{.cpp}
Dali::Handle::IsPropertyAnimatable()
~~~

that property can be animated using:

~~~{.cpp}
Dali::Animation
~~~

### Notifications

Using DALi's signal framework, applications can be notified when the animation finishes:

~~~{.cpp}

void ExampleCallback( Animation& source )
{
  std::cout << "Animation has finished" << std::endl;
}
...
animation.FinishedSignal().Connect( &ExampleCallback );
~~~

### Alpha Functions

Alpha functions are used in animations to specify the rate of change of the animation parameter over time.
The built in supported functions can be viewed in Dali::AlphaFunction::BuiltinFunction.

It is possible to specify a different alpha function for each animator in an Animation object:
~~~{.cpp}
animation.AnimateTo( Property( actor1, Dali::Actor::Property::POSITION ), Vector3(10.0f, 50.0f, 0.0f), Dali::AlphaFunction::EASE_IN );
~~~

### Other Actions

An animation can be looped:
~~~{.cpp}
animation.SetLooping( true );
~~~

By default, when an animation ends, the properties that it was animating are BAKED.
However, the property changes can be **discarded** when the animation ends (or is stopped):
~~~{.cpp}
animation.SetEndAction( Animation::Discard );
~~~

## Key-Frame Animation {#animation-key-frame}

DALi provides support for animating between several different values, i.e. key-frames.
A key frame takes a progress value between 0.0f and 1.0f (0 and 100% respectively) and portrays the value of the property when the animation has progressed by that amount.

See Programming guide for further information

## Path Animations {#animation-paths}

A Dali::Path can be used to animate the position and orientation of actors.

![ ](animation/animated-path.png)

The black points in the diagram are points where the DALi logo will travel to.
The red points are the control points which express the curvature of the path on the black points.

## Further Reading

### Programming guide

### Dali demos - Animation

There are two specific DALi demo 'examples':

~~~{.cpp}
animated-shapes.example
animated-images.example
~~~

Reminder - THe DALi environment variables must be sourced by either adding them to the .bashrc file
or running the following commands after opening a new terminal:
~~~{.cpp}
In the DALI parent directory (~/Public/DALI on my system)
dali-env/opt/bin/dali_env -s > setenv
. setenv
~~~

The source code for these examples can be found in:
~~~{.cpp}
../dali-demo/examples
~~~




