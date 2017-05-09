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
label.AnchorPoint = AnchorPoint.TopLeft;
Window.GetCurrent().Add( label );
~~~


The label must also be added to the main Window, or to a View which is on the main Window.  
The position of the label on-screen is dependent on the *parentOrigin* and *anchorPoint* properties.  

|  |  |
|--|--|
| (ParentOrigin.TOP_LEFT, AnchorPoint.TOP_LEFT) | ![ ](TextLabelTopLeft.png)   |

### Font Selection

By default the TextLabel will automatically select a suitable font from the platform. Note that the selected font
may not support all characters in your input text. For example, latin fonts often do not provide Arabic glyphs.

Alternatively you can request a font using the following properties:
~~~{.cs}
label.FONT_FAMILY = "Courier";
label.FONT_STYLE = "Regular";
label.POINT_SIZE = 12.0f;
~~~

The [Font Selection](@ref font-selection) section describes how to select a differant font in detail.

### Mark-up Style

Mark-up tags can be used to change the style of the text. See the [Mark-up Style](@ref markup-style) section for more details.

### Text Alignment

Wrapping can be enabled using the MULTI_LINE property:

~~~{.cs}
label.MULTI_LINE = true;
~~~

The text can be aligned horizontally to the beginning, end, or center of the available area:

~~~{.cs}
label.HORIZONTAL_ALIGNMENT = "BEGIN";
label.HORIZONTAL_ALIGNMENT = "CENTER";
label.HORIZONTAL_ALIGNMENT = "END";
~~~

|  |  |
|--|--|
| Here is the "BEGIN" alignment shown for left-to-right (Latin)   |  right-to-left (Arabic) scripts |
| ![ ](LatinBegin.png) | ![ ](ArabicBegin.png) |
| Here is the "CENTER" alignment shown for left-to-right (Latin)  | right-to-left (Arabic) scripts:|
| ![ ](LatinCenter.png) | ![ ](ArabicCenter.png) |
| Here is the "END" alignment shown for left-to-right (Latin)  | right-to-left (Arabic) scripts:|
| ![ ](LatinEnd.png) | ![ ](ArabicEnd.png) |


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
label.AnchorPoint( AnchorPoint.TOP_LEFT );
label.SetResizePolicy( ResizePolicyType.USE_NATURAL_SIZE, DimensionType.ALL_DIMENSIONS );
label.BackgroundColor( Color.BLUE );
Window.GetCurrent().Add( label );
~~~

 ![ ](HelloWorld-NaturalSize.png)


### Height-for-width negotiation

To layout text labels vertically, a fixed (maximum) width should be provided by the parent control.  
Each TextLabel will then report a desired height for the given width.  
Here is an example of this behavior using TableView as the parent:

~~~{.cs}
TableView parent = new TableView( 3, 1 );
parent.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
parent.SetResizePolicy( ResizePolicyType.USE_NATURAL_SIZE, DimensionType.HEIGHT );
parent.SetAnchorPoint( AnchorPoint.TOP_LEFT );
WIndow.GetCurrent().Add( parent );

TextLabel label = new TextLabel("Hello World");
label.AnchorPoint( AnchorPoint.TOP_LEFT );
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyType.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.BLUE );
parent.AddChild( label, new TableView.CellPosition( 0, 0 ) );
parent.SetFitHeight( 0 );

label = new TextLabel( "A Quick Brown Fox Jumps Over The Lazy Dog" );
label.AnchorPoint( AnchorPoint.TOP_LEFT );
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyType.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.GREEN );
label.MULTI_LINE = true;
parent.AddChild( label, new TableView.CellPosition( 1, 0 ) );
parent.SetFitHeight( 1 );

label = new TextLabel( "لإعادة ترتيب الشاشات، يجب تغيير نوع العرض إلى شبكة قابلة للتخصيص." );
label.SetAnchorPoint( AnchorPoint::TOP_LEFT );
label.SetResizePolicy( ResizePolicyType.FILL_TO_PARENT, DimensionType.WIDTH );
label.SetResizePolicy( ResizePolicyTYpe.DIMENSION_DEPENDENCY, DimensionType.HEIGHT );
label.BackgroundColor( Color.BLUE );
label.MULTI_LINE = true;
parent.AddChild( label, new TableView.CellPosition( 2, 0 ) );
parent.SetFitHeight( 2 );
~~~

 ![ ](HelloWorld-HeightForWidth.png)


Note that the "Hello World" text label (above) has been given the full width, not the natural width.

### TextLabel Decorations

#### Color

To change the color of the text, the recommended way is to use the TEXT_COLOR property.  
Note that unlike the Actor::COLOR property, this will not affect child Actors added to the TextLabel.  

~~~{.cS}
label.TEXT = "Red Text";
label.TEXT_COLOR = Color.RED;
~~~

 ![ ](RedText.png)

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

![ ](PlainText.png)

![ ](TextWithShadow.png)

![ ](TextWithBiggerShadow.png)

![ ](TextWithColorShadow.png)


#### Underline

The text can be underlined by setting UNDERLINE_ENABLED.  
The color can be selected using the UNDERLINE_COLOR property.  

~~~{.cs}
label1.TEXT = "Text with Underline";
label1.UNDERLINE_ENABLED = true;

label2.TEXT = "Text with Color Underline";
label2.UNDERLINE_ENABLED = true;
label2.UNDERLINE_COLOR = Color.GREEN;
~~~

![ ](TextWithUnderline.png)

![ ](TextWithColorUnderline.png)

By default the underline height will be taken from the font metrics, however this can be overridden using the UNDERLINE_HEIGHT property:

~~~{.cs}
label1.UNDERLINE_HEIGHT = 1.0f;
~~~

![ ](TextWith1pxUnderline.png)

### Auto Scrolling

![ ](AutoScroll.gif)

The \link text-auto-scrolling Auto text scrolling \endlink section details how to scroll text automatically.

### Text Label Properties

The properties used by TextLabel are listed [here](@ref TextLabelProperties)

