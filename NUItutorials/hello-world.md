# NUI Hello World Tutorial

<a name="0">
This tutorial provides an introduction to NUI and the DALi Animation framework.

The tutorial shows how to create and display "Hello World" using a text label.

A brief description of the key [NUI application class](#1) is also given.

[The NUI Overview](NUIoverview.md) describes the relationship between NUI and DALi in detail.

## Explanation of tutorial

The following steps are required to display text:

+ Initialise the DALi library
+ Create a View - a text label showing text
+ Add the text label to the application main window

### Creation

The application is derived from the Tizen NUI application class - _NUIApplication_

~~~{.cs}
Example example = new Example();
~~~

### Initialisation

An initialisation callback is added to the application _Initialized_ event.

~~~{.cs}
example.Initialized += Initialize;
~~~

### The Initialisation callback - Initialize

Get main application window and add callback to window _Touch_ event.
Hence event handler invoked on any click in application window.

~~~{.cs}
Window window = Window.Instance;
window.Touch += OnWindowTouched;
~~~

Position text in centre of application window.

~~~{.cs}
text.ParentOrigin = ParentOrigin.Center;
text.AnchorPoint  = AnchorPoint.Center;
~~~

The ParentOrigin defines a point within the parent views's area.
The AnchorPoint	 defines a point within the child views's area (_the label_)

Align text horizontally to the center of the available area.

~~~{.cs}
text.HorizontalAlignment = HorizontalAlignment.Center;
~~~

Set text size. The size of the font in points.

~~~{.cs}
text.PointSize = 32.0f;
~~~

Add text to default layer.

_Layers provide a mechanism for overlaying groups of views on top of each other._

~~~{.cs}
window.GetDefaultLayer().Add(text);
~~~

### main loop

To run the application, its main loop should be started. This ensues that images are displayed, events and signals
are displatched and captured.

~~~{.cs}
example.Run(args);
~~~

### quit application

Click anywhere in application window to exit.

~~~{.cs}

private void OnWindowTouched(object sender, WIndow.TouchEventArgs e)
{
   example.Application.Quit();
}
~~~

## Full example code

~~~{.cs}

namespace HelloTest
{
    class Example : NUIApplication
    {
        private void Initialize(object src, EventArgs e)
        {
            // Connect the signal callback for stage touched signal
            Window window = Window.Instance;
            window.Touch += OnWindowTouched;

            // Add a simple text label to the main window
            TextLabel text = new TextLabel("Hello DALi World");
            text.ParentOrigin = ParentOrigin.Center;
            text.AnchorPoint  = AnchorPoint.Center;
            text.HorizontalAlignment = HorizontalAlignment.Center;
            text.PointSize = 32.0f;

            window.GetDefaultLayer().Add(text);
        }

        // Callback for main window touched signal handling
        private void OnWindowTouched(object sender, WIndow.TouchEventArgs e)
        {
	    example.Application.Quit();
        }

        static void Main(string[] args)
        {
            Tizen.Log.Debug("NUI", "Hello world.");

            Example example = new Example();
	    example.Initialized += Initialize;
            example.Run(args);
        }
    }
}
~~~

[Back to top](#0)<br>

<a name="#1">
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

#### appiication window

The application window instance.

~~~{.cs}
private Window _window;
~~~

#### Cnnstructors

~~~{.cs}
/// The default constructor.
public NUIApplication() : base()

/// The constructor with stylesheet
public NUIApplication(string stylesheet) : base()

/// The constructor with stylesheet and window mode.
public NUIApplication(string stylesheet, WindowMode windowMode) : base()pMode.StyleSheetOnly;
~~~

### More information on the Text label 

The [Text Label tutorial](text-label.md) describes the key properties of the text label in detail.

