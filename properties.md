
# Properties and constraints

## What is a property?

A property is a value used by an object that can be modified or read externally to that object.
This modification could be from within DALi or externally by an application. The key feature
of properties is that they can be dynamically modified at run time.

Properties can be used to expose useful information or behaviour of a View. However care should be exercised as which variables can be exposed, to limit the public interface.

Classes such as Views have pre-defined Properties. User defined Custom properties can also be defined.

## What is a constraint?

A constraint makes a property a function of other properties.

## What is a notification ?

An application can get a _notification_ when a property reaches a threshold value.

## What is a property used for ?

Properties can be set externally by an application, allowing that application to change the configuration or behaviour of a view.
This could include the physical geometry of the view, or how it is drawn or moves.

Properties can also be read. This feature can be used in conjunction with constraints to allow changes to a property within one view to change the property of another view. For example, a view following the movement of a separate view (that it is not a child of).



## How to implement a property within DALi Core:

+ Define the properties as an enum in the public-api header file.
+ Define the property details using methods to build up a table of property information.

Methods exist which are designed to help with and standardise the definition of the propery details table per class.
These methods generate an array of property details which allow efficient lookup of flags like "animatable" or "constraint input".

An example of a property can be seen in **within the public-api header file Layer.h**

The internal implementation canbe seen in **layer-impl.cpp**:


Specific methods are used to define properties for the following reasons:

- To standardise the way properties are defined.
- To handle type-registering for properties, signals and actions in one place.
- To facilitate the posibility of running the code with the type-registry disabled.

Seperate methods are provided for event-side only property or animatable property.


<hr>
**Property Indices**

The properties are enumerated to give them a unique index. This index can be used to access them.
The indecies must be unique per flattened derivation heirachy.

EG:
- CameraActor derives from Actor. No property indicies in either CameraActor or Actor should collide with each other.
- ActiveConstraintBase derives from Object. It CAN have property indices that match Actor or CameraActor.

There are some predefined start indices and ranges that should be used for common cases, these are defined below:


DALi has a property system and provides several different kinds of properties. These are DEfault, Registered, Control, Derived Control
Registered Animatable and Custom. An index range is used for each of the the different properties in place.


Common uses for properties are constraints and animations.


<br>
<hr>
**Property use in JavaScript**

Note that constraints cannot be used within JavaScript, so below is a simple example that sets one of the default properties; scale:

<br>
<hr>
**Property use in JSON**


*
*/
