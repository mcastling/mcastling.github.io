# Image View Tutorial

The tutorial describes the NUI _ImageView_ control in detail.

## Overview

The ImageView is a control which displays an image.  

### Basic creation and usage

An instance of an ImageView is created by a URL: 

~~~{.cs}
 imageView = new ImageView("./images/gallery-3.jpg");
~~~

or:

~~~{.cs}
 imageView = new ImageView();
 imageView.ResourceUrl = "./images/gallery-3.jpg";
~~~

To subsequently change an image use the _SetImage_ method:

~~~{.cs}
 imageView.SetImage("./images/gallery-4.jpg");
~~~
 
### Image View Properties

ImageView has the following properties:

|  Property enum  | Property  | Type |
| ------------ | ------------ | ------------ |
| RESOURCE_URL | ResourceUrl | string  |
| IMAGE | ImageMap | Map 
| PRE_MULTIPLIED_ALPHA | PreMUltipliedAlpha | bool |
| PIXEL_AREA | PixelArea | Vector4 |

A brief description of each property:

+ ResourceUrl        - path to image file.
+ ImageMap           - multiple properties associated with a given image.
+ PreMUltipliedAlpha - If true, the RGB components represent the color of the object or pixel adjusted for its opacity by multiplication.
                     - if false, the RGB components represent the colour of the object or pixel, disregarding its opacity.
+ PixelArea          - sub area of image.

