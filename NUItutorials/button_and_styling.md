# NUI button and styling Tutorial

<a name="0">
This tutorial provides an introduction 


## Explanation of tutorial



[Back to top](#0)<br>

<a name="#1"></a>
### Notes on the NUIApplication class and its members

The **NUIApplication** class represents application that have a UI screen, in addition this class has a default __Window__

The following NUIApplication class member variables are of particular interest:

#### DALi application class

The NUIApplication class is in effect a wrapper class around the DALi application class.

~~~{.cs}
private Application _application;
~~~

#### Style sheets

DALi control properties are stylable. The stylesheet contains json markup for the controls.

Style sheets can be written at the device level i.e seperate stylesheets for mobiles and TV's. The stylesheets
are passed in at the NUI application class constructors level.

~~~{.cs}
private string _stylesheet;
~~~

#### window mode

The window mode can be Opaque or Transparent.

~~~{.cs}
private Application.WindowMode _windowMode;
~~~

#### application mode

The application mode can be Default, StyleSheetOnly or StyleSheetWithWindowMode.

~~~{.cs}
private AppMode _appMode;
~~~

#### application window

The application window instance.

~~~{.cs}
private Window _window;
~~~

#### Constructors

~~~{.cs}
/// The default constructor.
public NUIApplication() : base()

/// The constructor with stylesheet
public NUIApplication(string stylesheet) : base()

/// The constructor with stylesheet and window mode.
public NUIApplication(string stylesheet, WindowMode windowMode) : base()pMode.StyleSheetOnly;
~~~



