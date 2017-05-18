# NUI overview (as at May 2017)

This overview provides an introduction to NUI and the DALi animation framework.

NUI - Natural User Interface.

NUI is a C# interface to DALi.

## What is DALi?

DALi - Dynamic Animation Library, a cross platform library for creating applications with rich GUI. These applications
are run on a range of Tizen devices. Dali is built on a multi-threaded architecture enabling realistic smooth animations.
In addition a range of optimisation techniques are utilised to obtain low CPU and GPU usage, further increasing graphics
performance.

DALi was originally developed in C++. DALi is now been developed in parallel in C#, currently all C# functionaility
is simply a wrapper around the existing native C++ API. It is conceivable in future that new UI elements may be developed
in C# that have no direct C++ version (_though would still use the native C++ API where required_).

## key NUI API interface changes

The NUI DALi API has been modified from its original C# base.

+ THe NUI API was changed in order to minimise the number of API's exposed but unrequired in C#. This was partially
  to do with the Handle/Body idiom which has to be exposed to application developers in C++ but is available by default
  in C#.

+ Actor has been removed, all actor properties have been moved to View. View is the basic building block for a UI. Views can
  can be added to other Views or Layers.

+ A layer no longer inherits from Actor. All relevant Actor properties (e.g. Position, size) that applied to layer, were moved into Layer as well.
  A Layer can only be added at the top level. It is no longer possible to add a Layer to a View (considered unnecessary).

+ Both Layer and View now inherit from a new Animatable class. An Animatable class's properties can be animated by using the
  Animation class.

+ Window and Stage have been combined solely into the Window class. To add a View or Layer to the scene, add them to the Window.
  The Window can be used to retrieve the list of Layers.

![ ](./Images/NewWindowHierarchy.png)

## Use of NUI applications

Currently NUI is been used in the development of TV's.

## Planned branch merge

SRUK devel/master branch will be merged with Tizen branch. At that point the intention is to cease seperate development of both
the SRUK DALi and the NUI Tizen.

