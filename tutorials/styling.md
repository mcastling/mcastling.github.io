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

or 2) via code using SetProperty()

~~~{.cpp}
Dali::Toolkit::Control control = Dali::Toolkit::Control::New();
control.SetProperty( Control::BACKGROUND, "file_name.png" );
~~~


The json approach means that there are no code changes and recompilation required, if the png file needs to be changed.


### Style sheets

Style sheets are written at the device level i.e seperate stylesheets for mobiles and TV's

An example of a style sheet can be seen .....


## Further Reading

### Programming guide

The Styling section

### Dali demos - Styling

There are two specific DALi demo 'examples':

~~~{.cpp}
styling.example
animated-images.example
~~~

Reminder - THe DALi environment variables must be sourced by either adding them to the .bashrc file
or running the following commands after opening a new terminal:
~~~{.cpp}
In the DALI parent directory (~/Public/DALI on my system)
dali-env/opt/bin/dali_env -s > setenv
. setenv
~~~

The source code for this example can be found in:
~~~{.cpp}
../dali-demo/examples/styling
~~~




