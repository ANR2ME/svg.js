## Referencing elements

### By id
If you want to get an element created by SVG.js by its id, you can use the `SVG.get()` method:

```javascript
var element = SVG.get('my_element')

element.fill('#f06')
```

### Using CSS selectors
There are two ways to select elements using CSS selectors.

The first is to search globally. This will search in all svg elements in a document and return them in an instance of `SVG.Set`:

```javascript
var elements = SVG.select('rect.my-class').fill('#f06')
```

The second is to search within a parent element:

```javascript
var elements = group.select('rect.my-class').fill('#f06')
```

### Using jQuery or Zepto
Another way is to use [jQuery](http://jquery.com/) or [Zepto](http://zeptojs.com/). Here is an example:

```javascript
// add elements
var draw   = SVG('drawing')
var group  = draw.group().addClass('my-group')
var rect   = group.rect(100,100).addClass('my-element')
var circle = group.circle(100).addClass('my-element').move(100, 100)

// get elements in group
var elements = $('#drawing g.my-group .my-element').each(function() {
  this.instance.animate().fill('#f09')
})
```

## Circular reference
Every element instance within SVG.js has a reference to the actual `node`:

### node
```javascript
element.node
```
__`returns`: `node`__

### native()
The same can be achieved with the `native()` method:
```javascript
element.native()
```
__`returns`: `node`__


### instance
Similar, the node carries a reference to the SVG.js `instance`:

```javascript
node.instance
```
__`returns`: `element`__

## Parent reference
Every element has a reference to its parent with the `parent()` method:

### parent()

```javascript
element.parent()
```

__`returns`: `element`__

Alternatively a class or css selector can be passed as the first argument:

```javascript
var draw   = SVG('drawing')
var nested = draw.nested().addClass('test')
var group  = nested.group()
var rect   = group.rect(100, 100)

rect.parent()           //-> returns group
rect.parent(SVG.Doc)    //-> returns draw
rect.parent(SVG.Nested) //-> returns nested
rect.parent(SVG.G)      //-> returns group
rect.parent('.test')    //-> returns nested
```

__`returns`: `element`__

Even the main svg document:

```javascript
var draw = SVG('drawing')

draw.parent() //-> returns the wrappig html element with id 'drawing'
```

__`returns`: `HTMLNode`__


### doc()
For retrieving the root svg you can use `doc()`

```javascript
var draw = SVG('drawing')
var rect = draw.rect(100, 100)

rect.doc() //-> returns draw
```

### parents()
To get all ancestors of the element filtered by type or css selector (see `parent()` method)

```javascript
var group1 = draw.group().addClass('test')
  , group2 = group1.group()
  , rect   = group2.rect(100,100)

rect.parents()        // returns [group1, group2, draw]
rect.parents('.test') // returns [group1]
rect.parents(SVG.G)   // returns [group1, group2]
```

__`returns`: `Array`__

## Child references

### first()
To get the first child of a parent element:

```javascript
draw.first()
```
__`returns`: `element`__

### last()
To get the last child of a parent element:

```javascript
draw.last()
```
__`returns`: `element`__

### children()
An array of all children will can be retreives with the `children` method:

```javascript
draw.children()
```
__`returns`: `array`__

### each()
The `each()` allows you to iterate over the all children of a parent element:

```javascript
draw.each(function(i, children) {
  this.fill({ color: '#f06' })
})
```

Deep traversing is also possible by passing true as the second argument:

```javascript
// draw.each(block, deep)
draw.each(function(i, children) {
  this.fill({ color: '#f06' })
}, true)
```

Note that `this` refers to the current child element.

__`returns`: `itself`__

### has()
Checking the existence of an element within a parent:

```javascript
var rect  = draw.rect(100, 50)
var group = draw.group()

draw.has(rect)  //-> returns true
group.has(rect) //-> returns false
```
__`returns`: `boolean`__

### index()
Returns the index of given element and returns -1 when it is not a child:

```javascript
var rect  = draw.rect(100, 50)
var group = draw.group()

draw.index(rect)  //-> returns 0
group.index(rect) //-> returns -1
```
__`returns`: `number`__

### get()
Get an element on a given position in the children array:

```javascript
var rect   = draw.rect(20, 30)
var circle = draw.circle(50)

draw.get(0) //-> returns rect
draw.get(1) //-> returns circle
```
__`returns`: `element`__

### clear()
To remove all elements from a parent element:

```javascript
draw.clear()
```
__`returns`: `itself`__


## Import / export SVG

### svg()
Exporting the full generated SVG, or a part of it, can be done with the `svg()` method:

```javascript
draw.svg()
```

Exporting works on all elements.

Importing is done with the same method:

```javascript
draw.svg('<g><rect width="100" height="50" fill="#f06"></rect></g>')
```

Importing works on any element that inherits from `SVG.Parent`, which is basically every element that can contain other elements.

`getter`__`returns`: `string`__

`setter`__`returns`: `itself`__

## Attributes and styles

### attr()
You can get and set an element's attributes directly using `attr()`.

Get a single attribute:
```javascript
rect.attr('x')
```

Set a single attribute:
```javascript
rect.attr('x', 50)
```

Set multiple attributes at once:
```javascript
rect.attr({
  fill: '#f06'
, 'fill-opacity': 0.5
, stroke: '#000'
, 'stroke-width': 10
})
```

Set an attribute with a namespace:
```javascript
rect.attr('x', 50, 'http://www.w3.org/2000/svg')
```

Explicitly remove an attribute:
```javascript
rect.attr('fill', null)
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__


### style()
With the `style()` method the `style` attribute can be managed like attributes with `attr`:

```javascript
rect.style('cursor', 'pointer')
```

Multiple styles can be set at once using an object:

```javascript
rect.style({ cursor: 'pointer', fill: '#f03' })
```

Or a css string:

```javascript
rect.style('cursor:pointer;fill:#f03;')
```

Similar to `attr()` the `style()` method can also act as a getter:

```javascript
rect.style('cursor')
// => pointer
```

Or even a full getter:

```javascript
rect.style()
// => 'cursor:pointer;fill:#f03;'
```

Explicitly deleting individual style definitions works the same as with the `attr()` method:

```javascript
rect.style('cursor', null)
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### fill()
The `fill()` method is a pretty alternative to the `attr()` method:

```javascript
rect.fill({ color: '#f06', opacity: 0.6 })
```

A single hex string will work as well:

```javascript
rect.fill('#f06')
```

Last but not least, you can also use an image as fill, simply by passing an image url:

```javascript
rect.fill('images/shade.jpg')
```

Or if you want more control over the size of the image, you can pass an image instance as well:

```javascript
rect.fill(draw.image('images/shade.jpg', 20, 20))
```

__`returns`: `itself`__

### stroke()
The `stroke()` method is similar to `fill()`:

```javascript
rect.stroke({ color: '#f06', opacity: 0.6, width: 5 })
```

Like fill, a single hex string will work as well:

```javascript
rect.stroke('#f06')
```

Not unlike the `fill()` method, you can also use an image as stroke, simply by passing an image url:

```javascript
rect.stroke('images/shade.jpg')
```

Or if you want more control over the size of the image, you can pass an image instance as well:

```javascript
rect.stroke(draw.image('images/shade.jpg', 20, 20))
```

__`returns`: `itself`__

### opacity()
To set the overall opacity of an element:

```javascript
rect.opacity(0.5)
```

__`returns`: `itself`__

### reference()
In cases where an element is linked to another element through an attribute, the linked element instance can be fetched with the `reference()` method. The only thing required is the attribute name:

```javascript
use.reference('href') //-> returns used element instance
// or
rect.reference('fill') //-> returns gradient or pattern instance for example
// or
circle.reference('clip-path') //-> returns clip instance
```

### hide()
Hide element:

```javascript
rect.hide()
```

__`returns`: `itself`__

### show()
Show element:

```javascript
rect.show()
```

__`returns`: `itself`__

### visible()
To check if the element is visible:

```javascript
rect.visible()
```

__`returns`: `boolean`__

## Classes

### classes()
Fetches the css classes for the node as an array:

```javascript
rect.classes()
```

`getter`__`returns`: `array`__

### hasClass()
Test the presence of a given css class:

```javascript
rect.hasClass('purple-rain')
```

`getter`__`returns`: `boolean`__

### addClass()
Adds a given css class:

```javascript
rect.addClass('pink-flower')
```

`setter`__`returns`: `itself`__

### removeClass()
Removes a given css class:

```javascript
rect.removeClass('pink-flower')
```

`setter`__`returns`: `itself`__

### toggleClass()
Toggles a given css class:

```javascript
rect.toggleClass('pink-flower')
```

`setter`__`returns`: `itself`__

## Size and position

While positioning an element by directly setting its attributes works only if the attributes are used natively by that type of element, the positioning methods described below are much more convenient as they work for all element types.

For example, the following code works because each element is positioned by setting native attributes:

```javascript
rect.attr({ x: 20, y: 60 })
circle.attr({ cx: 50, cy: 40 })
```

The `rect` will be moved by its upper left corner to the new coordinates, and the `circle` will be moved by its center. However, trying to move a `circle` by its 'corner' or a `rect` by its center in this way will fail. The following lines will get silently ignored as the attributes that are addressed are not natively used by the element setting them:

```javascript
rect.attr({ cx: 20, cy: 60 })
circle.attr({ x: 50, y: 40 })
```

However, the positioning methods detailed below will work for all element types, regardless of whether the attributes being addressed are native to the type. So, unlike the lines above, these lines work just fine:

```javascript
rect.cx(20).cy(60)
circle.x(50).y(40)
```

It is important to note, though, that these methods are only intended for use with user (unitless) coordinates. If, for example, an element has its size set via percentages or other units, the positioning methods that address its native attributes will most likely still work, but the ones that address non-native attributes will give unexpected results -- as both getters and setters!


### size()
Set the size of an element to a given `width` and `height`:

```javascript
rect.size(200, 300)
```

Proportional resizing is also possible by leaving out `height`:

```javascript
rect.size(200)
```

Or by passing `null` as the value for `width`:

```javascript
rect.size(null, 200)
```

As with positioning, the size of an element could be set by using `attr()`. But because every type of element is handles its size differently the `size()` method is much more convenient.

There is one exception though: for `SVG.Text` elements, this method takes only one argument and applies the given value to the `font-size` attribute.

__`returns`: `itself`__

### width()
Set the width of an element:

```javascript
rect.width(200)
```

This method also acts as a getter:

```javascript
rect.width() //-> returns 200
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### height()
Set the height of an element:

```javascript
rect.height(325)
```

This method also acts as a getter:

```javascript
rect.height() //-> returns 325
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### radius()
Circles, ellipses, and rects may use the `radius()` method. On rects, it defines rounded corners.

For a circle, the argument sets the `r` attribute.

```javascript
circle.radius(10)
```

For ellipses and rects, pass two arguments to set the `rx` and `ry` attributes individually. Or, pass a single argument, to make the two attributes equal.

```javascript
ellipse.radius(10, 20)
rect.radius(5)
```

_This functionality requires the sugar.js module which is included in the default distribution._

__`returns`: `itself`__

### move()
Move the element by its upper left corner to a given `x` and `y` position:

```javascript
rect.move(200, 350)
```

__`returns`: `itself`__

### x()
Move the element by its upper left corner along the x-axis only:

```javascript
rect.x(200)
```

Without an argument the `x()` method serves as a getter as well:

```javascript
rect.x() //-> returns 200
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### y()
Move the element by its upper left corner along the y-axis only:

```javascript
rect.y(350)
```

Without an argument the `y()` method serves as a getter as well:

```javascript
rect.y() //-> returns 350
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### center()
Move the element by its center to a given `cx` and `cy` position:

```javascript
rect.center(150, 150)
```

__`returns`: `itself`__

### cx()
Move the element by its center in the `x` direction only:

```javascript
rect.cx(200)
```

Without an argument the `cx()` method serves as a getter as well:

```javascript
rect.cx() //-> returns 200
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### cy()
Move the element by its center in the `y` direction only:

```javascript
rect.cy(350)
```

Without an argument the `cy()` method serves as a getter as well:

```javascript
rect.cy() //-> returns 350
```

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### dmove()
Shift the element in both the `x` and `y` directions relative to its current position:

```javascript
rect.dmove(10, 30)
```

__`returns`: `itself`__

### dx()
Shift the element in the `x` direction relative to its current position:

```javascript
rect.dx(200)
```

__`returns`: `itself`__

### dy()
Shift the element in the `y` direction relative to its current position:

```javascript
rect.dy(200)
```

__`returns`: `itself`__

## Document tree manipulations

### clone()
To make an exact copy of an element the `clone()` method comes in handy:

```javascript
var clone = rect.clone()
```

__`returns`: `element`__

This will create a new, unlinked copy. For making a linked clone, see the [use](#use) element.
By default the cloned element is placed directly after the orginal element.
When you pass a parent parameter to the `clone()` function it will append the cloned element to the given parent.

### remove()
Removes the calling element from the svg document:

```javascript
rect.remove()
```

__`returns`: `itself`__

### replace()
At the calling element's position in the svg document, replace the calling element with the element passed to the method.

```javascript
rect.replace(draw.circle(100))
```

__`returns`: `element`__


### add()
Sets the calling element as the parent node of the argument. Returns the parent:

```javascript
var rect = draw.rect(100, 100)
var group = draw.group()

group.add(rect) //-> returns group
```

__`returns`: `itself`__

### put()
Sets the calling element as the parent node of the argument. Returns the child:

```javascript
group.put(rect) //-> returns rect
```

__`returns`: `element`__

### addTo()
Sets the calling element as a child node of the argument. Returns the child:

```javascript
rect.addTo(group) //-> returns rect
```

__`returns`: `itself`__

### putIn()
Sets the calling element as a child node of the argument. Returns the parent:

```javascript
rect.putIn(group) //-> returns group
```

__`returns`: `element`__

### toParent()
Moves an element to a different parent (similar to `addTo`), but without changing its visual representation. All transformations are merged and applied to the element.

```javascript
rect.toParent(group) // looks the same as before
```

__`returns`: `itself`__

### toDoc()
Same as `toParent()` but with the root-node as parent

__`returns`: `itself`__

### ungroup() / flatten()
Break up a group/container and move all the elements to a given parent node without changing their visual representations. The result is a flat svg structure, e.g. for exporting.

```javascript
// ungroups all elements in this group recursively and places them into the given parent
// (default: parent container of the calling element)
group.ungroup(parent, depth)

// call it on the whole document to get a flat svg structure
drawing.ungroup()

// breaks up the group and places all elements in drawing
group.ungroup(drawing)

// breaks up all groups until it reaches a depth of 3
drawing.ungroup(null, 3)

// flat and export svg
var svgString = drawing.ungroup().svg()
```

__`returns`: `itself`__


## Transforms

### transform()

The `transform()` method acts as a full getter without an argument:

```javascript
element.transform()
```

The returned __`object`__ contains the following values:

- `x` (translation on the x-axis)
- `y` (translation on the y-axis)
- `skewX` (calculated skew on x-axis)
- `skewY` (calculated skew on y-axis)
- `scaleX` (calculated scale on x-axis)
- `scaleY` (calculated scale on y-axis)
- `rotation` (calculated rotation)
- `cx` (last used rotation centre on x-axis)
- `cy` (last used rotation centre on y-axis)

Additionally a string value for the required property can be passed:

```javascript
element.transform('rotation')
```

In this case the returned value is a __`number`__.


As a setter it has two ways of working. By default transformations are absolute. For example, if you call:

```javascript
element.transform({ rotation: 125 }).transform({ rotation: 37.5 })
```

The resulting rotation will be `37.5` and not the sum of the two transformations. But if that's what you want there is a way out by adding the `relative` parameter. That would be:


```javascript
element.transform({ rotation: 125 }).transform({ rotation: 37.5, relative: true })
```

Alternatively a relative flag can be passed as the second argument:

```javascript
element.transform({ rotation: 125 }).transform({ rotation: 37.5 }, true)
```

Available transformations are:

- `rotation` with optional `cx` and `cy`
- `scale` with optional `cx` and `cy`
- `scaleX` with optional `cx` and `cy`
- `scaleY` with optional `cx` and `cy`
- `skewX` with optional `cx` and `cy`
- `skewY` with optional `cx` and `cy`
- `x`
- `y`
- `a`, `b`, `c`, `d`, `e` and/or `f` or an existing matrix instead of the object

`getter`__`returns`: `value`__

`setter`__`returns`: `itself`__

### rotate()
The `rotate()` method will automatically rotate elements according to the center of the element:

```javascript
// rotate(degrees)
rect.rotate(45)
```

Although you can also define a specific rotation point:

```javascript
// rotate(degrees, cx, cy)
rect.rotate(45, 50, 50)
```

__`returns`: `itself`__

### skew()
The `skew()` method will take an `x` and `y` value:

```javascript
// skew(x, y)
rect.skew(0, 45)
```

__`returns`: `itself`__

### scale()
The `scale()` method will take an `x` and `y` value:

```javascript
// scale(x, y)
rect.scale(0.5, -1)
```

__`returns`: `itself`__

### translate()
The `translate()` method will take an `x` and `y` value:

```javascript
// translate(x, y)
rect.translate(0.5, -1)
```


## Geometry

### viewbox()

The `viewBox` attribute of an `<svg>` element can be managed with the `viewbox()` method. When supplied with four arguments it will act as a setter:

```javascript
draw.viewbox(0, 0, 297, 210)
```

Alternatively you can also supply an object as the first argument:

```javascript
draw.viewbox({ x: 0, y: 0, width: 297, height: 210 })
```

Without any arguments an instance of `SVG.ViewBox` will be returned:

```javascript
var box = draw.viewbox()
```

But the best thing about the `viewbox()` method is that you can get the zoom of the viewbox:

```javascript
var box = draw.viewbox()
var zoom = box.zoom
```

If the size of the viewbox equals the size of the svg drawing, the zoom value will be 1.

`getter`__`returns`: `SVG.ViewBox`__

`setter`__`returns`: `itself`__

### bbox()
Get the bounding box of an element. This is a wrapper for the native `getBBox()` method but adds more values:

```javascript
path.bbox()
```

This will return an instance of `SVG.BBox` containing the following values:

- `width` (value from native `getBBox`)
- `height` (value from native `getBBox`)
- `w` (shorthand for `width`)
- `h` (shorthand for `height`)
- `x` (value from native `getBBox`)
- `y` (value from native `getBBox`)
- `cx` (center `x` of the bounding box)
- `cy` (center `y` of the bounding box)
- `x2` (lower right `x` of the bounding box)
- `y2` (lower right `y` of the bounding box)

The `SVG.BBox` has one other nifty little feature, enter the `merge()` method. With `merge()` two `SVG.BBox` instances can be merged into one new instance, basically being the bounding box of the two original bounding boxes:

```javascript
var box1 = draw.rect(100,100).move(50,50)
var box2 = draw.rect(100,100).move(200,200)
var box3 = box1.merge(box2)
```

__`returns`: `SVG.BBox`__

### tbox()
Where `bbox()` returns a bounding box mindless of any transformations, the `tbox()` method does take transformations into account. So any translation or scale will be applied to the resulting values to get closer to the actual visual representation:

```javascript
path.tbox()
```

This will return an instance of `SVG.TBox` containing the following values:

- `width` (value from native getBBox influenced by the `scaleX` of the current matrix)
- `height` (value from native getBBox influenced by the `scaleY` of the current matrix)
- `w` (shorthand for `width`)
- `h` (shorthand for `height`)
- `x` (value from native getBBox influenced by the `x` of the current matrix)
- `y` (value from native getBBox influenced by the `y` of the current matrix)
- `cx` (center `x` of the bounding box)
- `cy` (center `y` of the bounding box)
- `x2` (lower right `x` of the bounding box)
- `y2` (lower right `y` of the bounding box)

Note that the rotation of the element will not be added to the calculation.

__`returns`: `SVG.TBox`__

### rbox()
Is similar to `bbox()` but will give you the box around the exact visual representation of the element, taking all transformations into account.

```javascript
path.rbox()
```

This will return an instance of `SVG.RBox` containing the following values:

- `width` (the actual visual width)
- `height` (the actual visual height)
- `w` (shorthand for `width`)
- `h` (shorthand for `height`)
- `x` (the actual visual position on the x-axis)
- `y` (the actual visual position on the y-axis)
- `cx` (center `x` of the bounding box)
- `cy` (center `y` of the bounding box)
- `x2` (lower right `x` of the bounding box)
- `y2` (lower right `y` of the bounding box)

__Important__: Mozilla browsers include stroke widths where other browsers do not. Therefore the resulting box might be different in Mozilla browsers. It is very hard to modify this behavior so for the time being this is an inconvenience we have to live with.

__`returns`: `SVG.RBox`__

### ctm()
Retreives the current transform matrix of the element relative to the nearest viewport parent:

```javascript
path.ctm()
```

__`returns`: `SVG.Matrix`__

### screenCTM()
Retreives the current transform matrix of the element relative to the screen:

```javascript
path.screenCTM()
```

__`returns`: `SVG.Matrix`__

### matrixify()
Merges all transformations of the element into one single matrix which is returned

```javascript
path.matrixify()
```

__`returns`: `SVG.Matrix`__


### point()
Transforms a point from screen coordinates to the elements coordinate system

```javascript
// e is some mouseevent
var point = path.point(e.screeX, e.screenY) // {x, y}
```

__`returns`: `SVG.Point`__


### inside()
To check if a given point is inside the bounding box of an element you can use the `inside()` method:

```javascript
var rect = draw.rect(100, 100).move(50, 50)

rect.inside(25, 30) //-> returns false
rect.inside(60, 70) //-> returns true
```

Note: the `x` and `y` positions are tested against the relative position of the element. Any offset on the parent element is not taken into account.

__`returns`: `boolean`__

### length()
Get the total length of a path element:

```javascript
var length = path.length()
```

__`returns`: `number`__

### pointAt()
Get point on a path at given length:

```javascript
var point = path.pointAt(105) //-> returns { x : 96.88497924804688, y : 58.062747955322266 }
```

__`returns`: `object`__


## Animating elements

### Animatable method chain
Note that the `animate()` method will not return the targeted element but an instance of SVG.FX which will take the following methods:

Of course `attr()`:
```javascript
rect.animate().attr({ fill: '#f03' })
```

The `x()`, `y()` and `move()` methods:
```javascript
rect.animate().move(100, 100)
```

And the `cx()`, `cy()` and `center()` methods:
```javascript
rect.animate().center(200, 200)
```

If you include the sugar.js module, `fill()`, `stroke()`, `rotate()`, `skew()`, `scale()`, `matrix()`, `opacity()`, `radius()` will be available as well:
```javascript
rect.animate().rotate(45).skew(25, 0)
```

You can also animate non-numeric unit values using the `attr()` method:
```javascript
rect.attr('x', '10%').animate().attr('x', '50%')
```

### easing
All available ease types are:

- `<>`: ease in and out
- `>`: ease out
- `<`: ease in
- `-`: linear
- `=`: external control
- a function

For the latter, here is an example of the default `<>` function:

```javascript
function(pos) { return (-Math.cos(pos * Math.PI) / 2) + 0.5 }
```

For more easing equations, have a look at the [svg.easing.js](https://github.com/wout/svg.easing.js) plugin.

### animate()
Animating elements is very much the same as manipulating elements, the only difference is you have to include the `animate()` method:

```javascript
rect.animate().move(150, 150)
```

The `animate()` method will take three arguments. The first is `duration`, the second `ease` and the third `delay`:

```javascript
rect.animate(2000, '>', 1000).attr({ fill: '#f03' })
```

Alternatively you can pass an object as the first argument:

```javascript
rect.animate({ ease: '<', delay: '1.5s' }).attr({ fill: '#f03' })
```

By default `duration` will be set to `1000`, `ease` will be set to `<>`.

You can chain multiple animations together by calling `animate` again:

```javascript
rect.animate({ ease: '<', delay: '1.5s' }).attr({ fill: '#f03' }).animate().dmove(50,50)
```

__`returns`: `SVG.FX`__

### delay()
Alternatively you can call `delay()` which will set an delay in ms before the next animation in the queue is run

```javascript
rect.animate({ ease: '<', delay: '1.5s' }).attr({ fill: '#f03' }).delay(500).animate().dmove(50,50)
```

### queue()
If you want to call a custom funtion between two chained animations, you simply can queue them up:

```javascript
rect.animate({ ease: '<', delay: '1.5s' }).attr({ fill: '#f03' }).queue(function(){

    this.target().fill('#000')
    this.dequeue() // dont forget to call dequeue when the queue should continue running

}).animate().dmove(50,50)
```

### pause()
Pausing an animations is fairly straightforward:

```javascript
rect.animate().move(200, 200)

rect.mouseover(function() { this.pause() })
```

__`returns`: `itself`__

### play()
Will start playing a paused animation:

```javascript
rect.animate().move(200, 200)

rect.mouseover(function() { this.pause() })
rect.mouseout(function() { this.play() })
```
__`returns`: `itself`__

### stop()
If you just want to stop an animation you can call the `stop()` method which has two optional arguments:

 - jumpToEnd: Sets the values to the end of the animation
 - clearQueue: Remove all items from queue

```javascript
rect.animate().move(200, 200)

rect.stop()
// or e.g.
rect.stop(true)
```

Stopping an animation is irreversable.

__`returns`: `itself`__


### finish()
This method finishes the whole animation chain. All values are set to their corresponding end values and every situation gets fullfilled

```javascript
rect.animate().move(200, 200).animate().dmove(50,50).size(300,400)

rect.finish() // rect at 250,250 with size 300,400
```

__`returns`: `itself`__

### loop()
By default the `loop()` method creates and eternal loop:

```javascript
rect.animate(3000).move(100, 100).loop()
```

But the loop can also be a predefined number of times:

```javascript
rect.animate(3000).move(100, 100).loop(3)
```

Loops go from beginning to end and start over again (`0->1.0->1.0->1.`).

There is also a reverse flag that should be passed as the second argument:

```javascript
rect.animate(3000).move(100, 100).loop(3, true)
```

Loops will then be completely reversed before starting over (`0->1->0->1->0->1.`).

__`returns`: `SVG.FX`__

### reverse()
Toggles the direction of the animation or sets it to a specific direction:

```javascript
// will run from 100,100 to rects initial position
rect.animate(3000).move(100, 100).reverse()

// sets direction to backwards
rect.animate(3000).move(100, 100).reverse(true)

// sets direction to forwards (same as not calling reverse ever)
rect.animate(3000).move(100, 100).reverse(false)
```

__`returns`: `SVG.FX`__

### during/duringAll()
If you want to perform your own actions during one/all animation you can use the `during()/duringAll()` method:

```javascript
var position
  , from = 100
  , to   = 300

rect.animate(3000).move(100, 100).during(function(pos, morph, eased, situation) {
  position = from + (to - from) * pos
})

// or
rect.animate(3000).move(100, 100).duringAll(function(pos, morph, eased, situation) {
  position = from + (to - from) * pos
})
```
Note that `pos` is `0` in the beginning of the animation and `1` at the end of the animation.

To make things easier a morphing function is passed as the second argument. This function accepts a `from` and `to` value as the first and second argument and they can be a number, unit or hex color:

```javascript
var ellipse = draw.ellipse(100, 100).attr('cx', '20%').fill('#333')

rect.animate(3000).move(100, 100).during(function(pos, morph, eased, situation) {
  // numeric values
  ellipse.size(morph(100, 200), morph(100, 50))

  // unit strings
  ellipse.attr('cx', morph('20%', '80%'))

  // hex color strings
  ellipse.fill(morph('#333', '#ff0066'))
})
```
The `eased` parameter contains the position after the easing function was applied.
The last parameter holds the current situation related to the current `during` call.
You can call `during()/duringAll()` multiple times to add more functions which should be executed.

__`returns`: `SVG.FX`__

### after/afterAll()
Furthermore, you can add callback methods using `after()/afterAll()`:

```javascript
rect.animate(3000).move(100, 100).after(function(situation) {
  this.animate().attr({ fill: '#f06' })
})

// or
rect.animate(3000).move(100, 100).afterAll(function() {
  this.animate().attr({ fill: '#f06' })
})
```

The function gets the situation which was finished as first parameter. This doesn't apply to afterAll where no parameter is passed
Note that the `after()/afterAll()` method will never be called if the animation is looping eternally.
You can call `after()/afterAll()` multiple times to add more functions which should be executed.

__`returns`: `SVG.FX`__

### once()
Finally, you can perform an action at a specific position only once.
Just pass the position and the function which should be executed to the `once` method.
You can also decide whether the position which is passed should be handled as position in time (not eased) or position in space (easing applied):

```javascript
// the 0.5 is handled as uneased value (you can omit the false)
rect.animate(3000).move(100, 100).once(0.5, function(pos, eased) {
  // do something
}, false)
```

```javascript
// the 0.5 is handled as eased value
rect.animate(3000).move(100, 100).once(0.5, function(pos, eased) {
  // do something
}, true)
```

The callback function gets the current position uneased and eased

__`returns`: `SVG.FX`__

### at()
Say you want to control the position of an animation with an external event, then the `at()` method will proove very useful:

```javascript
var animation = draw.rect(100, 100).move(50, 50).animate('=').move(200, 200)

document.onmousemove = function(event) {
  animation.at(event.clientX / 1000)
}
```

The value passed as the first argument of `at()` should be a number between `0` and `1`, `0` being the beginning of the animation and `1` being the end. Note that any values below `0` and above `1` will be normalized.
Also note that the value is eased after calling the function. Therefore the position specifies a position in time not in space.

_This functionality requires the fx.js module which is included in the default distribution._

__`returns`: `SVG.FX`__

### target()
The target method returns the element the animation is applied to:

```javascript
rect.fx.target() // returns rect
```

`getter`__`returns`: `SVG.Element`__


### situation
The current situation of an animation is stored in the `situation` object:

```javascript
rect.animate(3000).move(100, 100)
rect.fx.situation //-> everything is in here
```

Available values are:

- `start` (start time as a number in milliseconds)
- `play` (animation playing or not; `true` or `false`)
- `pause` (time when the animation was last paused)
- `duration` (the chosen duration of the animation)
- `ease` (the chosen easing calculation)
- `finish` (start + duration)
- `loop` (the current loop; counting down if a number; `true`, `false` or a number)
- `loops` (if a number, the total number loops; `true`, `false` or a number)
- `reverse` (whether or not the animation should run backwards)
- `reversing` (`true` if the loop is currently reversing, otherwise `false`)


## Masking elements

### maskWith()
The easiest way to mask is to use a single element:

```javascript
var ellipse = draw.ellipse(80, 40).move(10, 10).fill({ color: '#fff' })

rect.maskWith(ellipse)
```

__`returns`: `itself`__

### mask()
But you can also use multiple elements:

```javascript
var ellipse = draw.ellipse(80, 40).move(10, 10).fill({ color: '#fff' })
var text = draw.text('SVG.JS').move(10, 10).font({ size: 36 }).fill({ color: '#fff' })

var mask = draw.mask().add(text).add(ellipse)

rect.maskWith(mask)
```

If you want the masked object to be rendered at 100% you need to set the fill color of the masking object to white. But you might also want to use a gradient:

```javascript
var gradient = draw.gradient('linear', function(stop) {
  stop.at({ offset: 0, color: '#000' })
  stop.at({ offset: 1, color: '#fff' })
})

var ellipse = draw.ellipse(80, 40).move(10, 10).fill({ color: gradient })

rect.maskWith(ellipse)
```

__`returns`: `SVG.Mask`__

### unmask()
Unmasking the elements can be done with the `unmask()` method:

```javascript
rect.unmask()
```

The `unmask()` method returns the masking element.

__`returns`: `itself`__

### remove()
Removing the mask alltogether will also `unmask()` all masked elements as well:

```javascript
mask.remove()
```

__`returns`: `itself`__

### masker
For your convenience, the masking element is also referenced in the masked element. This can be useful in case you want to change the mask:

```javascript
rect.masker.fill('#fff')
```

_This functionality requires the mask.js module which is included in the default distribution._


## Clipping elements
Clipping elements works exactly the same as masking elements. The only difference is that clipped elements will adopt the geometry of the clipping element. Therefore events are only triggered when entering the clipping element whereas with masks the masked element triggers the event. Another difference is that masks can define opacity with their fill color and clipPaths don't.

### clipWith()
```javascript
var ellipse = draw.ellipse(80, 40).move(10, 10)

rect.clipWith(ellipse)
```

__`returns`: `itself`__

### clip()
Clip multiple elements:

```javascript
var ellipse = draw.ellipse(80, 40).move(10, 10)
var text = draw.text('SVG.JS').move(10, 10).font({ size: 36 })

var clip = draw.clip().add(text).add(ellipse)

rect.clipWith(clip)
```

__`returns`: `SVG.ClipPath`__

### unclip()
Unclipping the elements can be done with the `unclip()` method:

```javascript
rect.unclip()
```

__`returns`: `itself`__

### remove()
Removing the clip alltogether will also `unclip()` all clipped elements as well:

```javascript
clip.remove()
```

__`returns`: `itself`__

### clipper
For your convenience, the clipping element is also referenced in the clipped element. This can be useful in case you want to change the clipPath:

```javascript
rect.clipper.move(10, 10)
```

_This functionality requires the clip.js module which is included in the default distribution._


## Arranging elements
You can arrange elements within their parent SVG document using the following methods.

### front()
Move element to the front:

```javascript
rect.front()
```

__`returns`: `itself`__

### back()
Move element to the back:

```javascript
rect.back()
```

__`returns`: `itself`__

### forward()
Move element one step forward:

```javascript
rect.forward()
```

__`returns`: `itself`__

### backward()
Move element one step backward:

```javascript
rect.backward()
```

__`returns`: `itself`__

### siblings()
The arrange.js module brings some additional methods. To get all siblings of rect, including rect itself:

```javascript
rect.siblings()
```

__`returns`: `array`__

### position()
Get the position (a number) of rect between its siblings:

```javascript
rect.position()
```

__`returns`: `number`__

### next()
Get the next sibling:

```javascript
rect.next()
```

__`returns`: `element`__

### previous()
Get the previous sibling:

```javascript
rect.previous()
```

__`returns`: `element`__

### before()
Insert an element before another:

```javascript
// inserts circle before rect
rect.before(circle)
```

__`returns`: `itself`__

### after()
Insert an element after another:

```javascript
// inserts circle after rect
rect.after(circle)
```

__`returns`: `itself`__

_This functionality requires the arrange.js module which is included in the default distribution._


## Sets
Sets are very useful if you want to modify or animate multiple elements at once. A set will accept all the same methods accessible on individual elements, even the ones that you add with your own plugins! Creating a set is exactly as you would expect:

```javascript
// create some elements
var rect = draw.rect(100,100)
var circle = draw.circle(100).move(100,100).fill('#f09')

// create a set and add the elements
var set = draw.set()
set.add(rect).add(circle)

// change the fill of all elements in the set at once
set.fill('#ff0')
```

A single element can be a member of many sets. Sets also don't have a structural representation, in fact they are just fancy array's.

### add()
Add an element to a set:

```javascript
set.add(rect)
```

Quite a useful feature of sets is the ability to accept multiple elements at once:

```javascript
set.add(rect, circle)
```

__`returns`: `itself`__

### each()
Iterating over all members in a set is the same as with svg containers:

```javascript
set.each(function(i) {
  this.attr('id', 'shiny_new_id_' + i)
})
```

Note that `this` refers to the current child element.

__`returns`: `itself`__

### has()
Determine if an element is member of the set:

```javascript
set.has(rect)
```

__`returns`: `boolean`__

### index()
Returns the index of a given element in the set.

```javascript
set.index(rect) //-> -1 if element is not a member
```

__`returns`: `number`__

### get()
Gets the element at a given index:

```javascript
set.get(1)
```

__`returns`: `element`__

### first()
Gets the first element:

```javascript
set.first()
```

__`returns`: `element`__

### last()
Gets the last element:

```javascript
set.last()
```

__`returns`: `element`__

### bbox()
Get the bounding box of all elements in the set:

```javascript
set.bbox()
```

__`returns`: `SVG.BBox`__

### remove()
To remove an element from a set:

```javascript
set.remove(rect)
```

__`returns`: `itself`__

### clear()
Or to remove all elements from a set:

```javascript
set.clear()
```

__`returns`: `itself`__

### animate()
Sets work with animations as well:

```javascript
set.animate(3000).fill('#ff0')
```

__`returns`: `SVG.SetFX`__


## Gradient

### gradient()
There are linear and radial gradients. The linear gradient can be created like this:

```javascript
var gradient = draw.gradient('linear', function(stop) {
  stop.at(0, '#333')
  stop.at(1, '#fff')
})
```

__`returns`: `SVG.Gradient`__

### at()
The `offset` and `color` parameters are required for stops, `opacity` is optional. Offset is float between 0 and 1, or a percentage value (e.g. `33%`).

```javascript
stop.at(0, '#333')
```

or

```javascript
stop.at({ offset: 0, color: '#333', opacity: 1 })
```

__`returns`: `itself`__

### from()
To define the direction you can set from `x`, `y` and to `x`, `y`:

```javascript
gradient.from(0, 0).to(0, 1)
```

The from and to values are also expressed in percent.

__`returns`: `itself`__

### to()
To define the direction you can set from `x`, `y` and to `x`, `y`:

```javascript
gradient.from(0, 0).to(0, 1)
```

The from and to values are also expressed in percent.

__`returns`: `itself`__

### radius()
Radial gradients have a `radius()` method to define the outermost radius to where the inner color should develop:

```javascript
var gradient = draw.gradient('radial', function(stop) {
  stop.at(0, '#333')
  stop.at(1, '#fff')
})

gradient.from(0.5, 0.5).to(0.5, 0.5).radius(0.5)
```

__`returns`: `itself`__

### update()
A gradient can also be updated afterwards:

```javascript
gradient.update(function(stop) {
  stop.at(0.1, '#333', 0.2)
  stop.at(0.9, '#f03', 1)
})
```

And even a single stop can be updated:

```javascript
var s1, s2, s3

draw.gradient('radial', function(stop) {
  s1 = stop.at(0, '#000')
  s2 = stop.at(0.5, '#f03')
  s3 = stop.at(1, '#066')
})

s1.update(0.1, '#0f0', 1)
```

__`returns`: `itself`__

### get()
The `get()` method makes it even easier to get a stop from an existing gradient:

```javascript
var gradient = draw.gradient('radial', function(stop) {
  stop.at({ offset: 0, color: '#000', opacity: 1 })   // -> first
  stop.at({ offset: 0.5, color: '#f03', opacity: 1 }) // -> second
  stop.at({ offset: 1, color: '#066', opacity: 1 })   // -> third
})

var s1 = gradient.get(0) // -> returns "first" stop
```

__`returns`: `SVG.Stop`__

### fill()
Finally, to use the gradient on an element:

```javascript
rect.attr({ fill: gradient })
```

Or:

```javascript
rect.fill(gradient)
```

By passing the gradient instance as the fill on any element, the `fill()` method will be called:

```javascript
gradient.fill() //-> returns 'url(#SvgjsGradient1234)'
```

[W3Schools](http://www.w3schools.com/svg/svg_grad_linear.asp) has a great example page on how
[linear gradients](http://www.w3schools.com/svg/svg_grad_linear.asp) and
[radial gradients](http://www.w3schools.com/svg/svg_grad_radial.asp) work.

_This functionality requires the gradient.js module which is included in the default distribution._

__`returns`: `value`__


## Pattern

### pattern()
Creating a pattern is very similar to creating gradients:

```javascript
var pattern = draw.pattern(20, 20, function(add) {
  add.rect(20,20).fill('#f06')
  add.rect(10,10)
  add.rect(10,10).move(10,10)
})
```

This creates a checkered pattern of 20 x 20 pixels. You can add any available element to your pattern.

__`returns`: `SVG.Pattern`__


### update()
A pattern can also be updated afterwards:

```javascript
pattern.update(function(add) {
  add.circle(15).center(10,10)
})
```

__`returns`: `itself`__


### fill()
Finally, to use the pattern on an element:

```javascript
rect.attr({ fill: pattern })
```

Or:

```javascript
rect.fill(pattern)
```

By passing the pattern instance as the fill on any element, the `fill()` method will be called on th pattern instance:

```javascript
pattern.fill() //-> returns 'url(#SvgjsPattern1234)'
```

__`returns`: `value`__


## Marker

### marker()
Markers can be added to every individual point of a `line`, `polyline`, `polygon` and `path`. There are three types of markers: `start`, `mid` and `end`. Where `start` represents the first point, `end` the last and `mid` every point in between.

```javascript
var path = draw.path('M 100 200 C 200 100 300  0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100z')

path.fill('none').stroke({ width: 1 })

path.marker('start', 10, 10, function(add) {
  add.circle(10).fill('#f06')
})
path.marker('mid', 10, 10, function(add) {
  add.rect(10, 10)
})
path.marker('end', 20, 20, function(add) {
  add.circle(6).center(4, 5)
  add.circle(6).center(4, 15)
  add.circle(6).center(16, 10)

  this.fill('#0f6')
})
```

The `marker()` method can be used in three ways. Firstly, a marker can be created on any container element (e.g. svg, nested, group, ...). This is useful if you plan to reuse the marker many times so it will create a marker in the defs but not show it yet:

```javascript
var marker = draw.marker(10, 10, function(add) {
  add.rect(10, 10)
})
```

Secondly a marker can be created and applied directly on its target element:

```javascript
path.marker('start', 10, 10, function(add) {
  add.circle(10).fill('#f06')
})
```

This will create a marker in the defs and apply it directly. Note that the first argument defines the position of the marker and that there are four arguments as opposed to three with the first example.

Lastly, if a marker is created for reuse on a container element, it can be applied directly on the target element:

```javascript
path.marker('mid', marker)
```

Finally, to get a marker instance from the target element reference:

```javascript
path.reference('marker-end')
```


### ref()
By default the `refX` and `refY` attributes of a marker are set to respectively half the `width` nd `height` values. To define the `refX` and `refY` of a marker differently:

```javascript
marker.ref(2, 7)
```

__`returns`: `itself`__

### update()
Updating the contents of a marker will `clear()` the existing content and add the content defined in the block passed as the first argument:

```javascript
marker.update(function(add) {
  add.circle(10)
})
```

__`returns`: `itself`__

### width()
Defines the `markerWidth` attribute:

```javascript
marker.width(10)
```

__`returns`: `itself`__

### height()
Defines the `markerHeight` attribute:

```javascript
marker.height(10)
```

__`returns`: `itself`__

### size()
Defines the `markerWidth` and `markerHeight` attributes:

```javascript
marker.size(10, 10)
```

__`returns`: `itself`__


## Data

### Setting
The `data()` method allows you to bind arbitrary objects, strings and numbers to SVG elements:

```javascript
rect.data('key', { value: { data: 0.3 }})
```

Or set multiple values at once:

```javascript
rect.data({
  forbidden: 'fruit'
, multiple: {
    values: 'in'
  , an: 'object'
  }
})
```

__`returns`: `itself`__

### Getting
Fetching the values is similar to the `attr()` method:

```javascript
rect.data('key')
```

__`returns`: `itself`__

### Removing
Removing the data altogether:

```javascript
rect.data('key', null)
```

__`returns`: `itself`__

### Sustaining data types
Your values will always be stored as JSON and in some cases this might not be desirable. If you want to store the value as-is, just pass true as the third argument:

```javascript
rect.data('key', 'value', true)
```

__`returns`: `itself`__


## Memory

### remember()
Storing data in-memory is very much like setting attributes:

```javascript
rect.remember('oldBBox', rect.bbox())
```

Multiple values can also be remembered at once:

```javascript
rect.remember({
  oldFill:    rect.attr('fill')
, oldStroke:  rect.attr('stroke')
})
```

To retrieve a memory

```javascript
rect.remember('oldBBox')
```

__`returns`: `itself`__

### forget()
Erasing a single memory:

```javascript
rect.forget('oldBBox')
```

Or erasing multiple memories at once:


```javascript
rect.forget('oldFill', 'oldStroke')
```

And finally, just erasing the whole memory:

```javascript
rect.forget()
```

__`returns`: `itself`__

## Events

### Basic events
Events can be bound to elements as follows:

```javascript
rect.click(function() {
  this.fill({ color: '#f06' })
})
```

Removing it is quite as easy:

```javascript
rect.click(null)
```

All available events are: `click`, `dblclick`, `mousedown`, `mouseup`, `mouseover`, `mouseout`, `mousemove`, `touchstart`, `touchmove`, `touchleave`, `touchend` and `touchcancel`.

__`returns`: `itself`__

### Event listeners
You can also bind event listeners to elements:

```javascript
var click = function() {
  this.fill({ color: '#f06' })
}

rect.on('click', click)
```

**Note:** The context of `this` in the callback is bound to the element. You can change this context by applying your own object:

```javascript
rect.on('click', click, window) // context of this is window
```

__`returns`: `itself`__

Unbinding events is just as easy:

```javascript
rect.off('click', click)
```

Or to unbind all listeners for a given event:

```javascript
rect.off('click')
```

Or even unbind all listeners for all events:

```javascript
rect.off()
```

__`returns`: `itself`__

But there is more to event listeners. You can bind events to html elements as well:

```javascript
SVG.on(window, 'click', click)
```

Obviously unbinding is practically the same:

```javascript
SVG.off(window, 'click', click)
```

### Custom events
You can even use your own events.

Just add an event listener for your event:
```javascript
rect.on('myevent', function() {
  alert('ta-da!')
})
```

Now you are ready to fire the event whenever you need:

```javascript
function whenSomethingHappens() {
  rect.fire('myevent')
}

// or if you want to pass an event
function whenSomethingHappens(event) {
  rect.fire(event)
}

```

You can also pass some data to the event:

```javascript
function whenSomethingHappens() {
  rect.fire('myevent', {some:'data'})
}

rect.on('myevent', function(e) {
  alert(e.detail.some) // outputs 'data'
})
```

svg.js supports namespaced events following the syntax `event.namespace`.

A namespaced event behaves like a normal event with the difference that you can remove it without touching handlers from other namespaces.

```
// attach
rect.on('myevent.namespace', function(e) {
  // do something
})

// detach all handlers of namespace for myevent
rect.off('myevent.namespace')

// detach all handlers of namespace
rect.off('.namespace')

// detach all handlers including all namespaces
rect.off('myevent)
```

However you can't fire a specific namespaced event. Calling `rect.fire('myevent.namespace')` won't do anything while `rect.fire('myevent')` works and fires all attached handlers of the event

_Important: always make sure you namespace your event to avoid conflicts. Preferably use something very specific. So `event.wicked` for example would be better than something generic like `event.svg`._

## Numbers

Numbers in SVG.js have a dedicated number class to be able to process string values. Creating a new number is simple:

```javascript
var number = new SVG.Number('78%')
number.plus('3%').toString() //-> returns '81%'
number.valueOf() //-> returns 0.81
```

Operators are defined as methods on the `SVG.Number` instance.

### plus()
Addition:

```javascript
number.plus('3%')
```

__`returns`: `SVG.Number`__

### minus()
Subtraction:

```javascript
number.minus('3%')
```

__`returns`: `SVG.Number`__

### times()
Multiplication:

```javascript
number.times(2)
```

__`returns`: `SVG.Number`__

### divide()
Division:

```javascript
number.divide('3%')
```

__`returns`: `SVG.Number`__

### to()
Change number to another unit:

```javascript
number.to('px')
```

__`returns`: `SVG.Number`__

### morph()
Make a number morphable:

```javascript
number.morph('11%')
```

__`returns`: `itself`__


### at()
Get morphable number at given position:

```javascript
var number = new SVG.Number('79%').morph('3%')
number.at(0.55).toString() //-> '37.2%'
```

__`returns`: `SVG.Number`__


## Colors

Svg.js has a dedicated color class handling different types of colors. Accepted values are:

- hex string; three based (e.g. #f06) or six based (e.g. #ff0066) `new SVG.Color('#f06')`
- rgb string; e.g. rgb(255, 0, 102) `new SVG.Color('rgb(255, 0, 102)')`
- rgb object; e.g. { r: 255, g: 0, b: 102 } `new SVG.Color({ r: 255, g: 0, b: 102 })`

Note that when working with objects is important to provide all three values every time.

The `SVG.Color` instance has a few methods of its own.

### toHex()
Get hex value:

```javascript
color.toHex() //-> returns '#ff0066'
```

__`returns`: hex color string__

### toRgb()
Get rgb string value:

```javascript
color.toRgb() //-> returns 'rgb(255,0,102)'
```

__`returns`: rgb color string__

### brightness()
Get the brightness of a color:

```javascript
color.brightness() //-> returns 0.344
```

This is the perceived brighness where `0` is black and `1` is white.

__`returns`: `number`__

### morph()
Make a color morphable:

```javascript
color.morph('#000')
```

__`returns`: `itself`__

### at()
Get morphable color at given position:

```javascript
var color = new SVG.Color('#ff0066').morph('#000')
color.at(0.5).toHex() //-> '#7f0033'
```

__`returns`: `SVG.Color`__


## Arrays
In SVG.js every value list string can be cast and passed as an array. This makes writing them more convenient but also adds a lot of key functionality to them.

### SVG.Array
Is for simple, whitespace separated value strings:

```javascript
'0.343 0.669 0.119 0 0 0.249 -0.626 0.13 0 0 0.172 0.334 0.111 0 0 0 0 0 1 0'
```

Can also be passed like this in a more manageable format:

```javascript
new SVG.Array([ .343,  .669, .119, 0,   0
              , .249, -.626, .130, 0,   0
              , .172,  .334, .111, 0,   0
              , .000,  .000, .000, 1,  -0 ])
```

### SVG.PointArray
Is a bit more complex and is used for polyline and polygon elements. This is a poly-point string:

```javascript
'0,0 100,100'
```

The dynamic representation:

```javascript
[
  [0, 0]
, [100, 100]
]
```

Precompiling it as an `SVG.PointArray`:

```javascript
new SVG.PointArray([
  [0, 0]
, [100, 100]
])
```

Note that every instance of `SVG.Polyline` and `SVG.Polygon` carries a reference to the `SVG.PointArray` instance:

```javascript
polygon.array() //-> returns the SVG.PointArray instance
```

_Javascript inheritance stack: `SVG.PointArray` < `SVG.Array`_

### SVG.PathArray
Path arrays carry arrays representing every segment in a path string:

```javascript
'M0 0L100 100z'
```

The dynamic representation:

```javascript
[
  ['M', 0, 0]
, ['L', 100, 100]
, ['z']
]
```

Precompiling it as an `SVG.PathArray`:

```javascript
new SVG.PathArray([
  ['M', 0, 0]
, ['L', 100, 100]
, ['z']
])
```

Note that every instance of `SVG.Path` carries a reference to the `SVG.PathArray` instance:

```javascript
path.array() //-> returns the SVG.PathArray instance
```

#### Syntax
The syntax for patharrays is very predictable. They are basically literal representations in the form of two dimentional arrays.

##### Move To
Original syntax is `M0 0` or `m0 0`. The SVG.js syntax `['M',0,0]` or `['m',0,0]`.

##### Line To
Original syntax is `L100 100` or `l100 100`. The SVG.js syntax `['L',100,100]` or `['l',100,100]`.

##### Horizontal line
Original syntax is `H200` or `h200`. The SVG.js syntax `['H',200]` or `['h',200]`.

##### Vertical line
Original syntax is `V300` or `v300`. The SVG.js syntax `['V',300]` or `['v',300]`.

##### Bezier curve
Original syntax is `C20 20 40 20 50 10` or `c20 20 40 20 50 10`. The SVG.js syntax `['C',20,20,40,20,50,10]` or `['c',20,20,40,20,50,10]`.

Or mirrored with `S`:

Original syntax is `S40 20 50 10` or `s40 20 50 10`. The SVG.js syntax `['S',40,20,50,10]` or `['s',40,20,50,10]`.

Or quadratic with `Q`:

Original syntax is `Q20 20 50 10` or `q20 20 50 10`. The SVG.js syntax `['Q',20,20,50,10]` or `['q',20,20,50,10]`.

Or a complete shortcut with `T`:

Original syntax is `T50 10` or `t50 10`. The SVG.js syntax `['T',50,10]` or `['t',50,10]`.

##### Arc
Original syntax is `A 30 50 0 0 1 162 163` or `a 30 50 0 0 1 162 163`. The SVG.js syntax `['A',30,50,0,0,1,162,163]` or `['a',30,50,0,0,1,162,163]`.

##### Close
Original syntax is `Z` or `z`. The SVG.js syntax `['Z']` or `['z']`.

The best documentation on paths can be found at https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths.


_Javascript inheritance stack: `SVG.PathArray` < `SVG.Array`_

### morph()
In order to animate array values the `morph()` method lets you pass a destination value. This can be either the string value, a plain array or an instance of the same type of SVG.js array:

```javascript
var array = new SVG.PointArray([[0, 0], [100, 100]])
array.morph('100,0 0,100 200,200')
```

This method will prepare the array ensuring both the source and destination arrays have the same length.

In order to morph paths you need to include the [svg.pathmorphing.js](https://github.com/wout/svg.pathmorphing.js) extension.

__`returns`: `itself`__

### at()
This method will morph the array to a given position between `0` and `1`. Continuing with the previous example:

```javascript
array.at(0.27).toString() //-> returns '27,0 73,100 127,127'
```

Note that this method is currently not available on `SVG.PathArray` but will be soon.

__`returns`: new instance__

### settle()
When morphing is done the `settle()` method will eliminate any transitional points like duplicates:

```javascript
array.settle()
```

Note that this method is currently not available on `SVG.PathArray` but will be soon.

__`returns`: `itself`__

### move()
Moves geometry of the array with the given `x` and `y` values:

```javascript
var array = new SVG.PointArray([[0, 0], [100, 100]])
array.move(33,75)
array.toString() //-> returns '33,75 133,175'
```

Note that this method is only available on `SVG.PointArray` and `SVG.PathArray`

__`returns`: `itself`__

### size()
Resizes geometry of the array by the given `width` and `height` values:

```javascript
var array = new SVG.PointArray([[0, 0], [100, 100]])
array.move(100,100).size(222,333)
array.toString() //-> returns '100,100 322,433'
```

Note that this method is only available on `SVG.PointArray` and `SVG.PathArray`

__`returns`: `itself`__

### reverse()
Reverses the order of the array:

```javascript
var array = new SVG.PointArray([[0, 0], [100, 100]])
array.reverse()
array.toString() //-> returns '100,100 0,0'
```

__`returns`: `itself`__

### bbox()
Gets the bounding box of the geometry of the array:

```javascript
array.bbox()
```

Note that this method is only available on `SVG.PointArray` and `SVG.PathArray`

__`returns`: `object`__


## Matrices
Matrices in SVG.js have their own class `SVG.Matrix`, wrapping the native `SVGMatrix`. They add a lot of functionality like extracting transform values, matrix morphing and improvements on the native methods.

### SVG.Matrix
In SVG.js matrices accept various values on initialization.

Without a value:

```javascript
var matrix = new SVG.Matrix
matrix.toString() //-> returns matrix(1,0,0,1,0,0)
```

Six arguments:

```javascript
var matrix = new SVG.Matrix(1, 0, 0, 1, 100, 150)
matrix.toString() //-> returns matrix(1,0,0,1,100,150)
```

A string value:

```javascript
var matrix = new SVG.Matrix('1,0,0,1,100,150')
matrix.toString() //-> returns matrix(1,0,0,1,100,150)
```

An object value:

```javascript
var matrix = new SVG.Matrix({ a: 1, b: 0, c: 0, d: 1, e: 100, f: 150 })
matrix.toString() //-> returns matrix(1,0,0,1,100,150)
```

A native `SVGMatrix`:

```javascript
var svgMatrix = svgElement.getCTM()
var matrix = new SVG.Matrix(svgMatrix)
matrix.toString() //-> returns matrix(1,0,0,1,0,0)
```

Even an instance of `SVG.Element`:

```javascript
var rect = draw.rect(50, 25)
var matrix = new SVG.Matrix(rect)
matrix.toString() //-> returns matrix(1,0,0,1,0,0)
```

### extract()
Gets the calculated values of the matrix as an object:

```javascript
matrix.extract()
```

The returned object contains the following values:

- `x` (translation on the x-axis)
- `y` (translation on the y-axis)
- `skewX` (calculated skew on x-axis)
- `skewY` (calculated skew on y-axis)
- `scaleX` (calculated scale on x-axis)
- `scaleY` (calculated scale on y-axis)
- `rotation` (calculated rotation)

__`returns`: `object`__

### clone()
Returns an exact copy of the matrix:

```javascript
matrix.clone()
```

__`returns`: `SVG.Matrix`__

### morph()
In order to animate matrices the `morph()` method lets you pass a destination matrix. This can be any value a `SVG.Matrix` would accept on initialization:

```javascript
matrix.morph('matrix(2,0,0,2,100,150)')
```

__`returns`: `itself`__

### at()
This method will morph the matrix to a given position between `0` and `1`:

```javascript
matrix.at(0.27)
```

This will only work when a destination matirx is defined using the `morph()` method.

__`returns`: `SVG.Matrix`__

### multiply()
Multiplies by another given matrix:

```javascript
matrix.matrix(matrix2)
```

__`returns`: `SVG.Matrix`__

### inverse()
Creates an inverted matix:

```javascript
matrix.inverse()
```

__`returns`: `SVG.Matrix`__

### translate()
Translates matrix by a given x and y value:

```javascript
matrix.translate(10, 20)
```

__`returns`: `SVG.Matrix`__

### scale()
Scales matrix uniformal with one value:

```javascript
// scale
matrix.scale(2)
```

Scales matrix non-uniformal with two values:

```javascript
// scaleX, scaleY
matrix.scale(2, 3)
```

Scales matrix uniformal on a given center point with three values:

```javascript
// scale, cx, cy
matrix.scale(2, 100, 150)
```

Scales matrix non-uniformal on a given center point with four values:

```javascript
// scaleX, scaleY, cx, cy
matrix.scale(2, 3, 100, 150)
```

__`returns`: `SVG.Matrix`__

### rotate()
Rotates matrix by degrees with one value given:

```javascript
// degrees
matrix.rotate(45)
```

Rotates a matrix by degrees around a given point with three values:

```javascript
// degrees, cx, cy
matrix.rotate(45, 100, 150)
```

__`returns`: `SVG.Matrix`__

### flip()
Flips matrix over a given axis:

```javascript
matrix.flip('x')
```

or

```javascript
matrix.flip('y')
```

By default elements are flipped over their center point. The flip axis position can be defined with the second argument:

```javascript
matrix.flip('x', 150)
```

or

```javascript
matrix.flip('y', 100)
```

__`returns`: `SVG.Matrix`__

### skew()
Skews matrix a given degrees over x and or y axis with two values:

```javascript
// degreesX, degreesY
matrix.skew(0, 45)
```

Skews matrix a given degrees over x and or y axis on a given point with four values:

```javascript
// degreesX, degreesY, cx, cy
matrix.skew(0, 45, 150, 100)
```

__`returns`: `SVG.Matrix`__

### around()
Performs a given matrix transformation around a given center point:

```javascript
// cx, cy, matrix
matrix.around(100, 150, new SVG.Matrix().skew(0, 45))
```

The matrix passed as the third argument will be used to multiply.

__`returns`: `SVG.Matrix`__

### native()
Returns a native `SVGMatrix` extracted from the `SVG.Matrix` instance:

```javascript
matrix.native()
```

__`returns`: `SVGMatrix`__

### toString()
Converts the matrix to a transform string:

```javascript
matrix.toString()
// -> matrix(1,0,0,1,0,0)
```

__`returns`: `string`__
