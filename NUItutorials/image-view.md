# Image View Tutorial

The tutorial describes the Dali ImageView control in detail.

## Overview

The Dali toolkit image view is a Dali control which displays an image.  

### Basic creation and usage

An instance of an image view is created by a URL: 

~~~{.cs}
 imageView = new ImageView("./images/gallery-3.jpg");
~~~

~~~{.cs}
 imageView = new ImageView();
 imageView.ResourceUrl = "./images/gallery-3.jpg";
~~~


To subsequently change an image use the SetImage function:

~~~{.cs}
 imageView.SetImage("./images/gallery-4.jpg");
~~~
 
### Image View Properties

ImageView has the following properties:

+ ResourceUrl		string        - path to image file
+ ImageMap           	Property Map  - multiple properties associated with a given image
+ PreMUltipliedAlpha 	bool	      - If true the RGB components represent the color of the object or pixel adjusted for its opacity by multiplication
                                      - if false the RGB components represent the colour of the object or pixel, disregarding its opacity
+ PixelArea     	Vector4       - sub area of image

