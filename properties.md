
# Properties and constraints

## What is a property?

A property is a value used by an object that can be modified or read externally to that object.
This modification could be from within DALi or externally by an application. The key feature
of properties is that they can be dynamically modified at run time.

Properties can be used to expose useful information or behaviour of a View. However care should be exercised as which variables can be exposed, to limit the public interface.

Classes such as Views have pre-defined Properties, such as Position, Size etc. User defined custom properties can also be defined.

Common uses for properties are constraints and animations.

## What is a constraint?

A constraint makes a property a function of other properties of other objects.

## What is a notification ?

An application can get a _notification_, which is a signal when a property reaches a threshold value.

## What is a property used for ?

Properties can be set externally by an application, allowing that application to change the configuration or behaviour of a view.
This could include the physical geometry of the view, or how it is drawn or moves.

Properties can also be read. This feature can be used in conjunction with constraints to allow changes to a property within one view to change the property of another view. For example, a view following the movement of a separate view (that it is not a child of).

<hr>

## Software considerations

### Property implementation

+ Define the properties as an enum in the public-api header file.
+ Define the property details using methods to build up a table of property information.

An example of a property can be seen in **the public-api header file Layer.h**

The internal implementation can be seen in **layer-impl.cpp**:

Specific methods are used to define properties for the following reasons:

- To standardise the way properties are defined.
- To handle type-registering for properties, signals and actions in one place.
- To facilitate the posibility of running the code with the type-registry disabled.

### Property Indices

Properties are enumerated to give them a unique index. 



