## Declaring Visuals to be used in your control as Properties

Visuals are the resources you use to build your control, they are defined in a style sheet and can be images, solid colors, models or text.

To link the Visual defined in the style sheet to your control you need to create a Property for it in your control.

Visual registration is done like regular Dali::Property registration, below example registers a visual called ICON_VISUAL with the control we are making; MyControl.  

Extract from header file and implementation cpp file.

The string name "iconVisual" is used by the style sheet.

The type for all Visuals is MAP

my-control.h

    namespace Property
    {
      enum Type
      {
        /**
         * @brief name "iconVisual", map
         * @details Sets the icon visual in MyControl
         */
        ICON_VISUAL,
        ...
      }
    }

my-control-impl.cpp

    DALI_DEVEL_PROPERTY_REGISTRATION( Toolkit, MyControl, "iconVisual", MAP, ICON_VISUAL  )

### Creating Visuals and Registering

There are two options to provide actual Visuals to the control, Using code and SetProperty or from the style sheet.

For either of these 2 methods the control must be able to create the Visual from the given Property Map.

Dali Toolkit provides a Visual Factory, passing the given Map to the VisualFactory::CreateVisual will return the Visual that matched the map.

        Toolkit::VisualFactory visualFactory = Toolkit::VisualFactory::Get();
        Property::Map *map = value.GetMap();
        if( map && !map->Empty() )
        {
          iconVisual = visualFactory.CreateVisual( *map );
        }

After creating a Visual, it should be Registered.  This will Add it to the control and allow for features like automatic StageConnection and Disconnection behavior.

        if ( iconVisual )
        {
          impl.RegisterVisual( index, iconVisual );
        }

index for the above is MyControl::Property::ICON_VISUAL

## Using code to provide Visuals for MyControl 

Below ICON_VISUAL is set to an Image with the url "application-icon-13.png".  
There are various ways to pass in the Property::Map but this one is longer but a good starting method.
  
    const char* ICON_IMAGE( "application-icon-13.png" );
    
    Property::Map iconMap;
    iconMap.Add(Toolkit::Visual::Property::TYPE, Toolkit::Visual::IMAGE );
    iconMap.Add(Toolkit::ImageVisual::Property::URL, ICON_IMAGE );
    
    myControl.SetProperty( Demo::ContentView::Property::ICON_VISUAL, iconMap );

## Using the style sheet to provide Visuals for MyControl.

The preferred way to provide Visuals is not in the code (as above) but via the style sheet, as shown below:

Below we provide an PNG to be used as the iconVisual in MyControl

my-style.json

    {
      "styles":
      {
        "MyControl":
        {
         "iconVisual":{
          "visualType":"IMAGE",
          "url":"/images/Logo-for-demo.png"
          }
        }
      }
    }
If another Visual was defined in MyControl, for example some Text, another Property would be defined and another entry added to style sheet.

my-control.h 

    namespace Property
    {
      enum Type
      {
        /**
         * @brief name "iconVisual", map
         * @details Sets the icon visual in MyControl
         */
        ICON_VISUAL,

         /**
         * @brief name "nameVisual", map
         * @details Sets the name visual in MyControl
         */
        NAME_VISUAL,
      }
    }

my-control-impl.cpp

    DALI_DEVEL_PROPERTY_REGISTRATION( Toolkit, MyControl, "iconVisual", MAP, ICON_VISUAL  )
    DALI_DEVEL_PROPERTY_REGISTRATION( Toolkit, MyControl, "nameVisual", MAP, NAME_VISUAL  )

my-style.json


    {
      "styles":
      {
        MyControl":
        {
         "iconVisual":{
            "visualType":"IMAGE",
            "url":"/images/Logo-for-dali.png"
          },
         "nameVisual":{
            "visualType":"TEXT",
            "text":"Start",
            "pointSize":20,
            "horizontalAlignment":"END",
            "textColor":[1,1,1,1],
          },
        }
      }
     }

## Adding Visuals to your control which change with NORMAL to FOCUSED states.

All Controls can have 3 states NORMAL, FOCUSED and DISABLED.

The visuals defined above apply to all states but states can have their own exclusive Visuals.

A set of Visuals can be provided for the NORMAL state and another set for the FOCUSED state.

When the control moves from NORMAL to FOCUSED the Visuals are changed depending on what is defined in the style sheet.

    "MyControl":
    {
      "states":
      {
        "NORMAL":
        {
          "visuals":
          {
            "iconVisual":
            {
              "url":"/images/application-icon-13.png"
            }
          }
        },
        "FOCUSED":
        {
          "visuals":
          {
            "iconVisual":
            {
              "url":"/images/application-icon-83.png"
            }
          }
        }
      }
    }
