### Basic Shapes

#### Rect
Rects have two arguments, their `width` and `height`:

```javascript
var rect = draw.rect(100, 100)
```

__`returns`: `SVG.Rect`__

_Javascript inheritance stack: `SVG.Rect` < `SVG.Shape` < `SVG.Element`_

##### radius()
Rects can also have rounded corners:

```javascript
rect.radius(10)
```

This will set the `rx` and `ry` attributes to `10`. To set `rx` and `ry` individually:

```javascript
rect.radius(10, 20)
```

__`returns`: `itself`__

#### Circle
The only argument necessary for a circle is the diameter:

```javascript
var circle = draw.circle(100)
```

__`returns`: `SVG.Circle`__

_Javascript inheritance stack: `SVG.Circle` < `SVG.Shape` < `SVG.Element`_

##### radius()
Circles can also be redefined by their radius:

```javascript
circle.radius(75)
```

__`returns`: `itself`__

#### Ellipse
Ellipses, like rects, have two arguments, their `width` and `height`:

```javascript
var ellipse = draw.ellipse(200, 100)
```

__`returns`: `SVG.Ellipse`__

_Javascript inheritance stack: `SVG.Ellipse` < `SVG.Shape` < `SVG.Element`_

##### radius()
Ellipses can also be redefined by their radii:

```javascript
ellipse.radius(75, 50)
```

__`returns`: `itself`__

#### Line
Create a line from point A to point B:

```javascript
var line = draw.line(0, 0, 100, 150).stroke({ width: 1 })
```

Creating a line element can be done in four ways. Look at the `plot()` method to see all the possiblilities.

__`returns`: `SVG.Line`__

_Javascript inheritance stack: `SVG.Line` < `SVG.Shape` < `SVG.Element`_

##### plot()
Updating a line is done with the `plot()` method:

```javascript
line.plot(50, 30, 100, 150)
```

Alternatively it also accepts a point string:

```javascript
line.plot('0,0 100,150')
```

Or a point array:

```javascript
line.plot([[0, 0], [100, 150]])
```

Or an instance of `SVG.PointArray`:

```javascript
var array = new SVG.PointArray([[0, 0], [100, 150]])
line.plot(array)
```

__`returns`: `itself`__

##### array()
References the `SVG.PointArray` instance. This method is rather intended for internal use:

```javascript
polyline.array()
```

__`returns`: `SVG.PointArray`__


#### Polyline
The polyline element defines a set of connected straight line segments. Typically, polyline elements define open shapes:

```javascript
// polyline('x,y x,y x,y')
var polyline = draw.polyline('0,0 100,50 50,100').fill('none').stroke({ width: 1 })
```

Polyline strings consist of a list of points separated by spaces: `x,y x,y x,y`.

As an alternative an array of points will work as well:

```javascript
// polyline([[x,y], [x,y], [x,y]])
var polyline = draw.polyline([[0,0], [100,50], [50,100]]).fill('none').stroke({ width: 1 })
```

__`returns`: `SVG.Polyline`__

_Javascript inheritance stack: `SVG.Polyline` < `SVG.Shape` < `SVG.Element`_

##### plot()
Polylines can be updated using the `plot()` method:

```javascript
polyline.plot([[0,0], [100,50], [50,100], [150,50], [200,50]])
```

The `plot()` method can also be animated:

```javascript
polyline.animate(3000).plot([[0,0], [100,50], [50,100], [150,50], [200,50], [250,100], [300,50], [350,50]])
```

__`returns`: `itself`__

##### array()
References the `SVG.PointArray` instance. This method is rather intended for internal use:

```javascript
polyline.array()
```

__`returns`: `SVG.PointArray`__

#### Polygon
The polygon element, unlike the polyline element, defines a closed shape consisting of a set of connected straight line segments:

```javascript
// polygon('x,y x,y x,y')
var polygon = draw.polygon('0,0 100,50 50,100').fill('none').stroke({ width: 1 })
```

Polygon strings are exactly the same as polyline strings. There is no need to close the shape as the first and last point will be connected automatically.

__`returns`: `SVG.Polygon`__

_Javascript inheritance stack: `SVG.Polygon` < `SVG.Shape` < `SVG.Element`_

##### plot()
Like polylines, polygons can be updated using the `plot()` method:

```javascript
polygon.plot([[0,0], [100,50], [50,100], [150,50], [200,50]])
```

The `plot()` method can also be animated:

```javascript
polygon.animate(3000).plot([[0,0], [100,50], [50,100], [150,50], [200,50], [250,100], [300,50], [350,50]])
```

__`returns`: `itself`__

##### array()
References the `SVG.PointArray` instance. This method is rather intended for internal use:

```javascript
polygon.array()
```

__`returns`: `SVG.PointArray`__

#### Path
The path string is similar to the polygon string but much more complex in order to support curves:

```javascript
draw.path('M 100 200 C 200 100 300  0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
```

__`returns`: `SVG.Path`__

_Javascript inheritance stack: `SVG.Path` < `SVG.Shape` < `SVG.Element`_

For more details on path data strings, please refer to the SVG documentation:
http://www.w3.org/TR/SVG/paths.html#PathData

##### plot()
Paths can be updated using the `plot()` method:

```javascript
path.plot('M100,200L300,400')
```

__`returns`: `itself`__

##### array()
References the `SVG.PathArray` instance. This method is rather intended for internal use:

```javascript
path.array()
```

__`returns`: `SVG.PathArray`__

#### Image
Creating images is as you might expect:

```javascript
var image = draw.image('/path/to/image.jpg')
```

If you know the size of the image, those parameters can be passed as the second and third arguments:

```javascript
var image = draw.image('/path/to/image.jpg', 200, 300)
```

__`returns`: `SVG.Image`__

_Javascript inheritance stack: `SVG.Image` < `SVG.Shape` < `SVG.Element`_

##### load()
Loading another image can be done with the `load()` method:

```javascript
image.load('/path/to/another/image.jpg')
```

__`returns`: `itself`__

##### loaded()
If you don't know the size of the image, obviously you will have to wait for the image to be `loaded`:

```javascript
var image = draw.image('/path/to/image.jpg').loaded(function(loader) {
  this.size(loader.width, loader.height)
})
```

The returned `loader` object as first the argument of the loaded method contains four values:
- `width`
- `height`
- `ratio` (width / height)
- `url`

__`returns`: `itself`__


#### Text
Unlike html, text in svg is much harder to tame. There is no way to create flowing text, so newlines should be entered manually. In SVG.js there are two ways to create text elements.

The first and easiest method is to provide a string of text, split by newlines:

```javascript
var text = draw.text("Lorem ipsum dolor sit amet consectetur.\nCras sodales imperdiet auctor.")
```

This will automatically create a block of text and insert newlines where necessary.

The second method will give you much more control but requires a bit more code:

```javascript
var text = draw.text(function(add) {
  add.tspan('Lorem ipsum dolor sit amet ').newLine()
  add.tspan('consectetur').fill('#f06')
  add.tspan('.')
  add.tspan('Cras sodales imperdiet auctor.').newLine().dx(20)
  add.tspan('Nunc ultrices lectus at erat').newLine()
  add.tspan('dictum pharetra elementum ante').newLine()
})
```

If you want to go the other way and don't want to add tspans at all, just one line of text, you can use the `plain()` method instead:

```javascript
var text = draw.plain('Lorem ipsum dolor sit amet consectetur.')
```

This is a shortcut to the `plain` method on the `SVG.Text` instance which doesn't render newlines at all.

_Javascript inheritance stack: `SVG.Text` < `SVG.Shape` < `SVG.Element`_

__`returns`: `SVG.Text`__

##### text()
Changing text afterwards is also possible with the `text()` method:

```javascript
text.text('Brilliant!')
```

__`returns`: `itself`__

To get the raw text content:

```javascript
text.text()
```

__`returns`: `string`__

##### tspan()
Just adding one tspan is also possible:

```javascript
text.tspan(' on a train...').fill('#f06')
```

__`returns`: `SVG.Tspan`__

##### plain()
If the content of the element doesn't need any stying or multiple lines, it might be sufficient to just add some plain text:

```javascript
text.plain('I do not have any expectations.')
```

__`returns`: `itself`__

##### font()
The sugar.js module provides some syntax sugar specifically for this element type:

```javascript
text.font({
  family:   'Helvetica'
, size:     144
, anchor:   'middle'
, leading:  '1.5em'
})
```

__`returns`: `itself`__

##### leading()
As opposed to html, where leading is defined by `line-height`, svg does not have a natural leading equivalent. In svg, lines are not defined naturally. They are defined by `<tspan>` nodes with a `dy` attribute defining the line height and a `x` value resetting the line to the `x` position of the parent text element. But you can also have many nodes in one line defining a different `y`, `dy`, `x` or even `dx` value. This gives us a lot of freedom, but also a lot more responsibility. We have to decide when a new line is defined, where it starts, what its offset is and what it's height is. The `leading()` method in SVG.js tries to ease the pain by giving you behaviour that is much closer to html. In combination with newline separated text, it works just like html:

```javascript
var text = draw.text("Lorem ipsum dolor sit amet consectetur.\nCras sodales imperdiet auctor.")
text.leading(1.3)
```

This will render a text element with a tspan element for each line, with a `dy` value of `130%` of the font size.

Note that the `leading()` method assumes that every first level tspan in a text node represents a new line. Using `leading()` on text elements containing multiple tspans in one line (e.g. without a wrapping tspan defining a new line) will render scrambeled. So it is advisable to use this method with care, preferably only when throwing newline separated text at the text element or calling the `newLine()` method on every first level tspan added in the block passed as argument to the text element.

__`returns`: `itself`__

##### build()
The `build()` can be used to enable / disable build mode. With build mode disabled, the `plain()` and `tspan()` methods will first call the `clear()` bethod before adding the new content. So when build mode is enabled, `plain()` and `tspan()` will append the new content to the existing content. When passing a block to the `text()` method, build mode is toggled automatically before and after the block is called. But in some cases it might be useful to be able to toggle it manually:


```javascript
var text = draw.text('This is just the start, ')

text.build(true)  // enables build mode

var tspan = text.tspan('something pink in the middle ').fill('#00ff97')
text.plain('and again boring at the end.')

text.build(false) // disables build mode

tspan.animate('2s').fill('#f06')
```

__`returns`: `itself`__

##### rebuild()
This is an internal callback that probably never needs to be called manually. Basically it rebuilds the text element whenerver `font-size` and `x` attributes or the `leading()` of the text element are modified. This method also acts a setter to enable or disable rebuilding:

```javascript
text.rebuild(false) //-> disables rebuilding
text.rebuild(true)  //-> enables rebuilding and instantaneously rebuilds the text element
```

__`returns`: `itself`__

##### clear()
Clear all the contents of the called text element:

```javascript
text.clear()
```

__`returns`: `itself`__

##### length()
Gets the total computed text length of all tspans together:

```javascript
text.length()
```

__`returns`: `number`__


##### lines()
All first level tspans can be referenced with the `lines()` method:

```javascript
text.lines()
```

This will return an intance of `SVG.Set` including all `tspan` elements.

__`returns`: `SVG.Set`__

##### events
The text element has one event. It is fired every time the `rebuild()` method is called:

```javascript
text.on('rebuild', function() {
  // whatever you need to do after rebuilding
})
```

#### Tspan
The tspan elements are only available inside text elements or inside other tspan elements. In SVG.js they have a class of their own:

_Javascript inheritance stack: `SVG.Tspan` < `SVG.Shape` < `SVG.Element`_

##### text()
Update the content of the tspan. This can be done by either passing a string:


```javascript
tspan.text('Just a string.')
```

Which will basicly call the `plain()` method.

Or by passing a block to add more specific content inside the called tspan:

```javascript
tspan.text(function(add) {
  add.plain('Just plain text.')
  add.tspan('Fancy text wrapped in a tspan.').fill('#f06')
  add.tspan(function(addMore) {
    addMore.tspan('And you can doo deeper and deeper...')
  })
})
```

__`returns`: `itself`__

##### tspan()
Add a nested tspan:

```javascript
tspan.tspan('I am a child of my parent').fill('#f06')
```

__`returns`: `SVG.Tspan`__

##### plain()
Just adds some plain text:

```javascript
tspan.plain('I do not have any expectations.')
```

__`returns`: `itself`__

##### dx()
Define the dynamic `x` value of the element, much like a html element with `position:relative` and `left` defined:

```javascript
tspan.dx(30)
```

__`returns`: `itself`__

##### dy()
Define the dynamic `y` value of the element, much like a html element with `position:relative` and `top` defined:

```javascript
tspan.dy(30)
```

__`returns`: `itself`__

##### newLine()
The `newLine()` is a convenience method for adding a new line with a `dy` attribute using the current "leading":

```javascript
var text = draw.text(function(add) {
  add.tspan('Lorem ipsum dolor sit amet ').newLine()
  add.tspan('consectetur').fill('#f06')
  add.tspan('.')
  add.tspan('Cras sodales imperdiet auctor.').newLine().dx(20)
  add.tspan('Nunc ultrices lectus at erat').newLine()
  add.tspan('dictum pharetra elementum ante').newLine()
})
```

__`returns`: `itself`__

##### clear()
Clear all the contents of the called tspan element:

```javascript
tspan.clear()
```

__`returns`: `itself`__

##### length()
Gets the total computed text length:

```javascript
tspan.length()
```

__`returns`: `number`__