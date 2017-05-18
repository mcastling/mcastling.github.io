# NUI Hello World Tutorial

<a name="0">
This tutorial provides an introduction to the NUI Animation framework.

The tutorial shows how to create and display "Hello World" using a text label.

[The NUI Overview](NUIoverview.md) describes NUI capabilities in detail.

## Explanation of tutorial

The following steps are required to display text:

+ Initialise the NUI library
+ Create a View - a text label showing text
+ Add the text label to the application main window

### Creation

The application is derived from the Tizen NUI application class - _NUIApplication_

~~~{.cs}
class Example : NUIApplication
~~~

### Initialisation

An initialisation callback is added to the application _Initialized_ event.

During initialisation the initial scene is set up.

The initialisation event is triggered once during the application lifetime.

~~~{.cs}
example.Initialized += Initialize;
~~~

### The Initialisation callback - Initialize

Create text label.

~~~{.cs}
TextLabel text = new TextLabel("Hello NUI World");
~~~

Get main application window and add callback to window _Touch_ event.
Hence this event handler is invoked on any click in the application window.

~~~{.cs}
Window window = Window.Instance;
window.Touch += OnWindowTouched;
~~~

Position text in centre of application window. Note the Text label will be at least the
width of the screen if the text label size is not specified. The ParentOrigin defines a point within the
parent views's area.

~~~{.cs}
text.ParentOrigin = ParentOrigin.CenterLeft;
~~~

Align text horizontally to the center of the available area.

~~~{.cs}
text.HorizontalAlignment = HorizontalAlignment.Center;
~~~

Set text label background color to illustrate label width.

~~~{.cs}
text.BackgroundColor = BackgroundColor = Color.Red;
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

### running application

To run the application, its main loop should be started. This ensues that images are displayed,
events and signals are displatched and captured.

~~~{.cs}
example.Run(args);
~~~

### Touch event handler

Click anywhere in the application window to exit.

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
	    text.BackgroundColor = Color.Red;
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

### More information on the Text label 

The [Text Label tutorial](text-label.md) describes the key properties of the text label in detail.

