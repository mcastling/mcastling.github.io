## How to style a control with json

### Styling Controls : Buttons

Use this guide to learn how to style a toolkit control with json.

Visual are the main building block of controls.  You will build your control from visuals by selecting the required ones and setting the attributes.

The following Visual types (visualType) are available:

| visualType  | example                    |  attributes |
| ------------- | ------------------------------ |---|
| Border     |  ![](https://github.com/dalihub/dali-toolkit/blob/master/docs/content/images/visuals/border-visual.png)    | borderColor, borderSize, antiAliasing |
| Color   | ![](https://github.com/dalihub/dali-toolkit/blob/master/docs/content/images/visuals/color-visual.png)  | mixColor     |
| Gradient   | ![](https://github.com/dalihub/dali-toolkit/blob/master/docs/content/images/visuals/linear-gradient-visual.png ) | startPosition, endPosition, center, radius, stopOffset, stopColor, units, spreadMethod |
| Image   | ![](https://github.com/dalihub/dali-demo/blob/master/resources/images/gallery-medium-50.jpg ) | url, desiredHeigth, desiredWidth |
| Mesh   | ![](https://github.com/dalihub/dali-toolkit/blob/master/docs/content/images/visuals/mesh-visual.png) | objectUrl, materialUrl, texturesPath, materialUrl   |
| Primitive   | ![](https://github.com/dalihub/dali-toolkit/blob/master/docs/content/images/visuals/cube.png)  | shape, mixColor,slices,stacks,scaleTopRadius, scaleHeight, scaleRadius, scaleDimension,bevelPercentage, bevelSmoothness,lightPosition   |
| Text   | **Hello there** | text,fontFamily, fontStyle, pointSize,multiLine,horizontalAlignment, verticalAlignment,textColor,enableMarkup    |

### Styling a PushButton

A PushButton is derived from Button which derives from Control which derives from Actor.<br />
Styling is inherited so styling a parent will automatically effect its child unless overridden.<br />
Actor, control and PushButton offer the following Style properties<br />

The button and control attributes are the most useful for styling

|  actor |    | control |   |  button |   |
| ------------  |------------ | ------------ | ------------ | ------------ | ------------ |
|  (mix)color | Vector4   | backgroundVisual | map or url | labelVisual  |  map or url  |
| colorAlpha  | float   |  |   | iconPadding  | vector4  |
| (mix)colorBlue   | float   |  |   | iconPadding  |  vector4 |
|  (mix)colorGreen | float   |  |   | labelRelativeAlignment  |  string |
|  (mix)colorRed | float  |   |   |  iconVisual | map or url  |
|  height[ResizePolicy](https://github.com/dalihub/test/wiki/Resize-Policy) | string    | |   |   |   |
| width[ResizePolicy](https://github.com/dalihub/test/wiki/Resize-Policy)  | string   |  |   |   |   |
|  sizeModeFactor | vector3   |  |   |   |   |
|  minimumSize | vector3  |   |   |   |   |
| maximumSize  | vector3  |   |   |   |   |


### Styling for State

Control has 3 states: NORMAL, FOCUSED and DISABLED. Each state should have the required visuals.

A different backgroundVisual can be supplied for NORMAL, FOCUSED and DISABLED if required.

Button has sub states: SELECTED and UNSELECTED

Each state can have its own set of visuals or a visual can be common between states. NORMAL, FOCUSED and DISABLED states will each have these sub-states.

You may want a different backgroundVisual for SELECTED and UNSELECTED state.

_"inherit"_ Push Button will inherit any styles defined for Button <br />
_"visuals"_ Visuals for specific states can be put here ( see next example ) <br />
_"states"_ Visuals for specific states can be put here ( see next example ) <br />

```json
"styles":
{
  "PushButton":
  {
    "inherit":["Button"],
    "visuals":
    {
      "iconVisual": { "visualType":"IMAGE", "url":"icon1.png" },
      "labelVisual": { "visualType":"TEXT", "text":"OK", "fontWeight":"bold" }
    },
    "states":
    {

    }
  }
}
```
### Building up a style sheet, state by state

1) All controls have have below states:

```json
  "states":
  {
    "NORMAL":
    {
    },
    "DISABLED":
    {
    },
    "FOCUSED":
    {
    },
  }
```
The states have been defined but no visuals provided.


2) Below the button offers the sub states UNSELECTED and SELECTED but still not visuals provided.

```json
  "states":
  {
    "NORMAL":
    {
      "states":
      {
        "UNSELECTED":
        {

        },
        "SELECTED":
        {

        }  
      },
    "DISABLED":
    {
      "states":
      {
        "UNSELECTED":
        {

        },
        "SELECTED":
        {

        }
      }
    },
    ...
  }
```
3) Now the background visual defined for each sub-state, the same can be done for FOCUSED and DISABLED.

```json
  "states":
  {
    "NORMAL":
    {
      "states":
      {
        "UNSELECTED":
        {
          "visuals":
          {
            "backgroundVisual":
             {
              "visualType":"IMAGE",
              "url":"backgroundUnSelected.png"
             }
          }
        },
        "SELECTED":
        {
          "visuals":
          {
            "backgroundVisual":
             {
              "visualType":"IMAGE",
              "url":"backgroundSelected.png"
             }
          }
        }
      }
    },
```

### Transitions

The control (Button) will change between states from user interaction. <br />

All controls can move between the states NORMAL, FOCUSED and DISABLED. <br />

Whilst in those states Button has sub-states SELECTED and UNSELECTED. <br />

To move between states and sub-states transition animations can be defined. <br />

Each state and sub-state can have an "entry" and "exit" transition. <br />

To make defining common transitions easier an effect can be used with a "from" and "to" state. <br />

One such effect is CROSSFADE which animates the opacity of visuals fading in and out to give a nice transition.
Initially only CROSSFADE will be available but in time further effect could be provided.

This transition can be placed in the state section like NORMAL. It will cross-fade between unselected and selected visuals. <br />

Example using CROSSFADE effect 
```json
"transitions":
[
  {
     "from":"UNSELECTED",
     "to":"SELECTED",
     "visualName":"*",
     "effect":"CROSSFADE",
     "animator":
     {
       "alphaFunction":"EASE_OUT",
       "duration":"0.2,
       "delay":0
     }
  }
]
```
Example using entry and exit transition for UNSELECTED
```json
"states":
  {
    "NORMAL":
    {
      "states":
      {
        "UNSELECTED":
        {
          "visuals":
          {
            "backgroundVisual":
             {
               "visualType":"IMAGE",
               "url":"backgroundUnSelected.png"
             }
          },
          "entryTransition":
          {
            "target":"backgroundVisual",
            "property":"mixColor",
            "targetValue":[1,1,1,1],
            "animator":
            {
              "alphaFunction":"LINEAR",
              "duration":0.3,
              "delay":0.0
            }
         },
         "exitTransition":
         {
            "target":"backgroundVisual",
            "property":"mixColor",
            "targetValue":[1,1,1,0.0],
            "animator":
            {
              "alphaFunction":"LINEAR",
              "duration":0.3,
              "delay":0.0
            }
         }
       }
     }
   }
  }
]
```



### Example Style sheet
Example button stylesheet  ( Link to most update version )
```json
{
  "styles":
  {
    "PushButton":
    {
      "inherit":["Button"],
      "visuals":
      {
        "iconVisual":
        {
          "visualType":"IMAGE",
          "url":"icon1.png"
        },
        "label":
        {
          "visualType":"TEXT",
          "text":"OK",
          "fontWeight":"bold"
        }
      },
      "states":
      {
        "NORMAL":
        {
          "states":
          {
            "UNSELECTED":
            {
              "visuals":
              {
                "backgroundVisual":
                {
                  "visualType":"IMAGE",
                  "url":"backgroundSelected.png"
                }
              }
            },
            "SELECTED":
            {
              "visuals":
              {
                "backgroundVisual":
                {
                  "visualType":"IMAGE",
                  "url":"backgroundUnselected.png"
                }
              }
            }
          },
          "transitions":
          [
            {
              "from":"UNSELECTED", 
              "to":"SELECTED",
              "visualName":"*",
              "effect":"CROSSFADE",
              "animator":
              {
                "alphaFunction":"EASE_OUT",
                "duration":0.2,
                "delay":0
              }
            }
          ]
        },
        "FOCUSED":
        {
          "visuals":
          {
            "labelVisual":
            {
              "visualType":"TEXT",
              "text":"OK",
              "fontWeight":"bold"
            }
          },
          "states":
          {
            "SELECTED":
            {
            },
            "UNSELECTED":
            {
            }
          }
        },
        "DISABLED":
        {
          "states":
          {
            "SELECTED":
            {
              "visuals":
              {
                "backgroundVisual":
                {
                  "visualType": "IMAGE",
                  "url": "{DALI_IMAGE_DIR}button-down-disabled.9.png"
                }
              }
            },
            "UNSELECTED":
            {
              "visuals":
              {
                "backgroundVisual":
                {
                  "visualType": "IMAGE",
                  "url": "{DALI_IMAGE_DIR}button-disabled.9.png"
                }
              }
            }
          },
          "transitions":
          {
            "visualName":"*",
            "effect":"CROSSFADE",
            "animator":
            {
              "alphaFunction":"EASE_IN_OUT",
              "duration":0.3
            }
          }
        }
      },
      "autoRepeating":false,
      "togglable":false,
      "labelPadding":[ 12.0, 12.0, 12.0, 12.0 ],

      "transitions":
      [
        {
          "from":"NORMAL", 
          "to":"DISABLED",
          "visualName":"*",
          "effect":"CROSSFADE",
          "animator":
          {
            "alphaFunction":"EASE_OUT",
            "duration":0.2,
            "delay":0
          }
        }
      ]
    }
  }
}
```

