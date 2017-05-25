# Styling Tutorial {#styling}

This tutorial provides an introduction to styling in DALi.

DALi control properties are stylable.

Controls can be styled both visually and behaviorally. A visual style change
would be the change of icon image for a change of state - button selected to unselected.

An example of a behavioural style would be text wrapping in a text edit control.

## Styling Controls



There are two approaches to styling a control, 1 is recommended.

1) json markup in one of the style sheet files.

~~~{.cpp}
      ...
      "control":
      {
        "filename":"{IMAGES}file_name.png"
      },
      ...
~~~

2) via code using SetProperty()

~~~{.cpp}
Dali::Toolkit::Control control = Dali::Toolkit::Control::New();
control.SetProperty( Control::BACKGROUND, "file_name.png" );
~~~


The json approach means that if a change is required (such as changing the png file), no code changes and the
subsequent recompilation are required.


### Style sheets

Style sheets can be written at the device level i.e. seperate stylesheets for mobiles and TV's.
The stylesheets are passed into the Application constructors.

Stlyesheets canalso be written at the control level. The default stylesheet read in on control construction.

An example of a style sheet can be seen .....


## Further Reading

### Programming guide

The Styling section.

### Dali demos - Styling

There are two specific DALi demo 'examples':

~~~{.cpp}
styling.example
animated-images.example
~~~

The source code for these examples can be found in:
~~~{.cpp}
../dali-demo/examples/styling
../dali-demo/examples/animated-images
~~~

Reminder - THe DALi environment variables must be sourced by either adding them to the .bashrc file
or running the following commands after opening a new terminal:
~~~{.cpp}
In the DALI parent directory (~/Public/DALI on my system)
dali-env/opt/bin/dali_env -s > setenv
. setenv
~~~




