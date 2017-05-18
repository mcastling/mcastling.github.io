# NUI overview (as at May 2017)

This overview provides an introduction to NUI and the DALi animation framework.

NUI  - Natural User Interface.
DALi - Dynamic Animation Library

NUI is a C# interface to the original DALi graphics library.

## Development of NUI

DALi was originally developed in C++. DALi was then subsequently developed in parallel in C#.
A seperate Tizen NUI branch was created from this C# DALi branch.

## NUI 

NUI is a cross platform library for creating applications with rich GUI. These applications are run on a
range of Tizen devices. Dali is built on a multi-threaded architecture enabling realistic smooth animations.
In addition a range of optimisation techniques are utilised to obtain low CPU and GPU usage, further
increasing graphics performance.

NUI enables developers to quickly create Rich UI applications with realistic effects and animations such as:

 + Image & Video galleries
 + Music players
 + Games
 + Maps
 + Homescreens / launch pads
 + Advanced watch faces for wearable devices

## NUI features

 + Maintains a hierarchical 3D scene graph consisting of parent and child nodes
 + Creates images & Text
 + Creates shaders using GLSL, each View has its own default shader
 + Provides multiple cameras and render targets
 + Provides Layers to aid in 2D UI layout
 + Automatic background loading of resources ( images / text / meshes )
 + Easy to use Animation framework using 3D Math.
 + Provides keyboard / touch / mouse handling

## Relationship Between NUI code and original DALi code

Currently all C# functionality is simply a wrapper around the existing native C++ API. It is conceivable in future
that new UI elements may be developed in C# that have no direct C++ version (_though would still use the native C++
API where required_).

## Planned branch merge

SRUK devel/master branch will be merged with the Tizen branch. At that point the intention is to cease seperate development of both
the SRUK DALi and the NUI Tizen.

