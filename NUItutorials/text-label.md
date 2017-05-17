# Text Label Tutorial

The tutorial describes the Dali TextLabel control in detail.

## Overview

The Dali TextLabel is a Dali control which renders a short text string.  
Text labels are lightweight, non-editable and do not respond to user input.

### Basic creation and usage

To display a TextLabel the TEXT property must be set using a UTF-8 string.

Note: *CR+LF* new line characters are replaced by *LF*.

~~~{.cs}
TextLabel label = new TextLabel("Hello World");
_window = Window.Instance;
_window.Add(label);
~~~


The label must also be added to the main Window, or to a View which is on the main Window.  
The position of the label on-screen is dependent on the *parentOrigin* and *anchorPoint* properties.  

|  |  |
|--|--|
| (ParentOrigin.TOP_LEFT, AnchorPoint.TOP_LEFT) | ![ ](../Images/TextLabelTopLeft.png)   |

<a name="#1"></a>
### Font Selection

By default the TextLabel will automatically select a suitable font from the platform. Note that the selected font
may not support all characters in your input text. For example, latin fonts often do not provide Arabic glyphs.

Alternatively you can request a font using either or all of FONT_FAMILY, FONT_STYLE, and POINT_SIZE properties:
~~~{.cs}
label.FontFamily = "FreeSerif";

Property.Map fontStyle = new Property.Map();
fontStyle.Add( "weight", "bold" )
         .Add( "slant", "italic" ) );

label.PointSize = 12.0f;

~~~

### Mark-up Style

Mark-up tags can be used to change the style of the text. 

By default the text controls don't process the mark-up string. To enable the mark-up string processing the property
*ENABLE_MARKUP* must be set to *true*.

~~~{.cs}
label.ENABLE_MARKUP = true;
~~~

Current supported tags are:

## \<color\>

Sets the color of the characters inside the tag. The *color* tag has a *value* attribute used to set the color.
Possible values are: 'red', 'green', 'blue', 'yellow', 'magenta', 'cyan', 'white', 'black' and 'transparent'.
Web color and 32 bits hexadecimal 0xAARRGGBB formats are also supported.

Examples below are equivalent, render the text in red. Second example codes the color in 0xAARRGGBB, third and fourth in web color with 3 and 6 characters.

~~~{.cs}
label.Text = "<color value='red'>Red Text</color>" ); // Color coded with a text constant.
~~~

~~~{.cs}
label.Text = "<color value='0xFFFF0000'>Red Text</color>" ); // Color packed inside an ARGB hexadecimal value.
~~~


## \<font\>

Sets the font values of the characters inside the tag.

Supported attributes are:
- *family* The name of the font.
- *size* The size of the font in points.
- *weight* The weight of the font.
- *width* The width of the font
- *slant* The slant of the font.

See [Font Selection](#1)

~~~{.cs}
label.Text = "<font family='SamsungSans' weight='bold'>Hello world</font>";
~~~


### Text Alignment

Wrapping can be enabled using the MULTI_LINE property:

~~~{.cs}
label.MULTI_LINE = true;
~~~

The text can be aligned horizontally to the beginning, end, or center of the available area:

~~~{.cs}
label.HorizontalAlignment = "BEGIN";
label.HorizontalAlignment = "CENTER";
label.HorizontalAlignment = "END";
~~~

|  |  |
|--|--|
| Here is the "BEGIN" alignment shown for left-to-right (Latin)   |  right-to-left (Arabic) scripts |
| ![ ](../Images/LatinBegin.png) | ![ ](../Images/ArabicBegin.png) |
| Here is the "CENTER" alignment shown for left-to-right (Latin)  | right-to-left (Arabic) scripts:|
| ![ ](../Images/LatinCenter.png) | ![ ](../Images/ArabicCenter.png) |
| Here is the "END" alignment shown for left-to-right (Latin)  | right-to-left (Arabic) scripts:|
| ![ ](../Images/LatinEnd.png) | ![ ](../Images/ArabicEnd.png) |


The examples above assume that the TextLabel size is greater than the minimum required.  
The next section provides details about the other size related options.

## Negotiating size

Size negotiation is a layouting feature supported by UI controls such as TextLabel, which allocates sizes for Views based on dependency rules.

The following examples show TextLabels actual size by setting a colored background, whilst the black area represents the size of the parent control:  

### Using natural size

With a "natural" size TextLabel will be large enough to display the text without wrapping, and will not have extra space to align the text within.  

Therefore in this example the same result would be displayed, regardless of the alignment or multi-line properties.  

~~~{.cs}
TextLabel label = new TextLabel("Hello World");
label.SetResizePolicy( ResizePolicyType.USE_NATURAL_SIZE, DimensionType.ALL_DIMENSIONS );
label.BackgroundColor( Color.BLUE );
Window window = Window.Instance;
window.Add( label );
~~~

 ![ ](../Images/HelloWorld-NaturalSize.png)


### Height-for-width negotiation

To layout text labels vertically, a fixed (maximum) width should be provided by the parent control.  
Each TextLabel will then report a desired height for the given width.  

Here is an example of this behavior using TableView as the parent:

~~~{.cs}
TableView parent = new TableView( 3, 1 );
parent.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
parent.SetResizePolicy( ResizePolicyType.USE_NATURAL_SIZE, DimensionType.HEIGHT );
WIndow.GetCurrent().Add( parent );

TextLabel label = new TextLabel("Hello World");
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyType.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.BLUE );
parent.AddChild( label, new TableView.CellPosition( 0, 0 ) );
parent.SetFitHeight( 0 );

label = new TextLabel( "A Quick Brown Fox Jumps Over The Lazy Dog" );
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyType.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.GREEN );
label.MultiLine = true;
parent.AddChild( label, new TableView.CellPosition( 1, 0 ) );
parent.SetFitHeight( 1 );

label = new TextLabel( "لإعادة ترتيب الشاشات، يجب تغيير نوع العرض إلى شبكة قابلة للتخصيص." );
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyTYpe.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.BLUE );
label.MultiLine = true;
parent.AddChild( label, new TableView.CellPosition( 2, 0 ) );
parent.SetFitHeight( 2 );
~~~

 ![ ](../Images/HelloWorld-HeightForWidth.png)


Note that the "Hello World" text label (above) has been given the full width, not the natural width.

### TextLabel Decorations

#### Color

To change the color of the text, the recommended way is to use the TEXT_COLOR property.  
Note that unlike the Actor::COLOR property, this will not affect child Actors added to the TextLabel.  

~~~{.cs}
label.TEXT = "Red Text";
label.TEXT_COLOR = Color.RED;
~~~

 ![ ](../Images/RedText.png)

#### Drop Shadow

To add a drop-shadow to the text, simply set the SHADOW property. Shadow parameters can be set through a json string, see the examples below.

~~~{.cs}
window.BackgroundColor( Color.BLUE );

label1.TEXT = "Plain Text";

label2.TEXT = "Text with Shadow";
label2.SHADOW = "{\"offset\":\"1 1\",\"color\":\"black\"}" );

label3.TEXT = "Text with Bigger Shadow";
label3.SHADOW = "{\"offset\":\"2 2\",\"color\":\"black\"}" );

label4.TEXT, "Text with Color Shadow" );
label4.SHADOW, "{\"offset\":\"1 1\",\"color\":\"red\"}" );
~~~

![ ](../Images/PlainText.png)

![ ](../Images/TextWithShadow.png)

![ ](../Images/TextWithBiggerShadow.png)

![ ](../Images/TextWithColorShadow.png)


#### Underline

The text can be underlined by setting UNDERLINE_ENABLED.  
The color can be selected using the UNDERLINE_COLOR property.  

~~~{.cs}
label1.Text = "Text with Underline";
label1.UnderlineEnabled = true;

label2.Text = "Text with Color Underline";
label2.UnderlineHeight = true;
label2.UnderlineColor = Color.GREEN;
~~~

![ ](../Images/TextWithUnderline.png)

![ ](../Images/TextWithColorUnderline.png)

By default the underline height will be taken from the font metrics, however this can be overridden using the
UNDERLINE_HEIGHT property:

~~~{.cs}
label1.UnderlineHeight = 1.0f;
~~~

![ ](../Images/TextWith1pxUnderline.png)

### Auto Scrolling

![ ](../Images/AutoScroll.gif)

Auto TextLabel scrolling enables the text to scroll within the control, it can be used if text exceeds the
boundary of the control hence showing the full content. Autoscrolling will also scroll text that is smaller than the control,
ensuring that the same bit of text is not visible at the same time, this gap can be configured to be larger.

The _loop count_ sets the number of scrolling repetitions i.e. if loop count is set to 3, scrolling of the text will occur 3 times.

If the loop count is not set, then once triggered to start, scrolling will continue until requested to stop.

Multiline text will not scroll.

The ENABLE_AUTO_SCROLL property (_EnableAutoScroll_) should be set to TRUE to enable scrolling.

~~~{.cs}
label.EnableAutoScroll = true;
~~~

Once enabled scrolling until the loop count is completed or the ENABLE_AUTO_SCROLL set to false. 
setting ENABLE_AUTO_SCROLL to false will let the text complete it's current scrolling loop then stop.

The scroll speed, gap and loop count can be set in the stylesheet or via these relevant properties:

#### AUTO_SCROLL_SPEED

_AutoScrollSpeed_ controls the speed of the scrolling, the speed should be provided as pixels/second.

#### AUTO_SCROLL_LOOP_COUNT

_AutoScrollLoopCount_ specifies how many times the text will complete a full scroll cycle.
If not set then scrolling will continue until ENABLE_AUTO_SCROLL is set to false.

Setting ENABLE_AUTO_SCROLL to false will stop scrolling whilst maintaining the original loop count value.

#### AUTO_SCROLL_GAP

This specifies the amount of whitespace to display before the scrolling text is shown again.

This will be increased if the given value is not large enough to prevent the same bit of text being visible at two locations in the control.

Provide the distance in pixels.

### Scroll Direction

The scroll direction is chosen automatically with the following rules:

If the text is single-lined it will scroll left when the text is Left to Right (LTR) or scroll right if text is Right to Left (RTL).

If the text is multi-lined it will scroll upwards.


### Text Label Properties

The properties used by TextLabel are listed [here](@ref TextLabelProperties)

