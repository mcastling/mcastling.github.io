# NUI overview (as at May 2017)

This overview provides an introduction to NUI and the DALi animation framework.

NUI is a graphics toolkit based on DALi.

## What is DALi?
DALi - Dynamic Animation Library

DALi is a cross platform library for creating applications with rich GUI. These applications
are run on a range of Tizen devices. Dali is built on a multi-threaded architecture enabling realistic smooth animations.
In addition a range of optimisation techniques are utilised to obtain low CPU and GPU usage, further increasing graphics
performance.

DALi was originally developed in C++. DALi is now been developed in parallel in C#, currently all C# functionaility
is simply a wrapper around the existing C++ API. It is concievable in future that new UI elements may be developed
in C# that have no direct C++ version (though would still use the native C++ API where required)

## What is NUI?
NUI - Natural User Interface

NUI is DALi plus modifications made by Tizen

Curremtly HQ are using NUI Inthe development of TV's.

## key NUI API interface changes

+ THe NUI API was recently changed in order to minimise the number of API's exposed but unrequired in C#. This was partially
  to do with the Handle/Body idiom which has to be exposed to application developers in c++ but is available by default
  in C# (almost everything is a handle in C#).

+ Actor has been removed, all actor properties have been moved to View. View is the basic building block for a UI. Views can
  can be added to other Views or Layers.

+ A layer no longer inherits from Actor. All relevant Actor properties (e.g. Position, size) that applied to layer, were moved into Layer as well.
  A Layer can only be added at the top level. It is no longer possible to add a Layer to a View (considered unnecessary).

+ Both Layer and View now inherit from a new Animatable class. An Animatable class's properties can be animated by using the
  Animation class.

+ Window and Stage have been combined solely into the Window class. To add a View or Layer to the scene, add them to the Window.
  The Window can be used to retrieve the list of Layers.

![ ](NewWindowHierarchy.png)

## History

2010 - present : DALi development in C++ (the 'native' API)
2017 - present : DALi development in C#
2017 - present : DALi toolkit development in C# (dali-toolkit folder) now includes two development folders 'dali-sharp' and 'dali-swig'.
                 The 'dali-sharp' folder contains the source code 'converted' to using __Views__ instead of __Actors__, and __WIndow__ instead
                 of __Stage__.

2016 : HQ 'froze' DALi C# development, started seperate development (Tizen branch). This is the NUI development
2016 - present : HQ developed further API's in NUI
2017 : HQ will merge SRUK devel/master branch with their own Tizen branch, to obtain SRUK changes in
       areas such as replacing actors and Stage (with Views and Window respectively). At that point
       the intention is to cease seperate development of both the SRUK DALi and the NUI Tizen development.

