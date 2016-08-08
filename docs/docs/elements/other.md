
#### TextPath
A nice feature in svg is the ability to run text along a path:

```javascript
var text = draw.text(function(add) {
  add.tspan('We go ')
  add.tspan('up').fill('#f09').dy(-40)
  add.tspan(', then we go down, then up again').dy(40)
})
text
  .path('M 100 200 C 200 100 300 0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
  .font({ size: 42.5, family: 'Verdana' })
```

When calling the `path()` method on a text element, the text element is mutated into an intermediate between a text and a path element. From that point on the text element will also feature a `plot()` method to update the path:

```javascript
text.plot('M 300 500 C 200 100 300 0 400 100 C 500 200 600 300 700 200 C 800 100 900 100 900 100')
```

Attributes specific to the `<textPath>` element can be applied to the textPath instance itself:

```javascript
text.textPath().attr('startOffset', 0.5)
```

And they can be animated as well of course:

```javascript
text.textPath().animate(3000).attr('startOffset', 0.8)
```

__`returns`: `SVG.TextPath`__

_Javascript inheritance stack: `SVG.TextPath` < `SVG.Element`_

##### textPath()
Referencing the textPath node directly:

```javascript
var textPath = text.textPath()
```

__`returns`: `SVG.TextPath`__

##### track()
Referencing the linked path element directly:

```javascript
var path = text.track()
```

__`returns`: `SVG.Path`__

#### Use
The use element simply emulates another existing element. Any changes on the master element will be reflected on all the `use` instances. The usage of `use()` is very straightforward:

```javascript
var rect = draw.rect(100, 100).fill('#f09')
var use  = draw.use(rect).move(200, 200)
```

In the case of the example above two rects will appear on the svg drawing, the original and the `use` instance. In some cases you might want to hide the original element. the best way to do this is to create the original element in the defs node:

```javascript
var rect = draw.defs().rect(100, 100).fill('#f09')
var use  = draw.use(rect).move(200, 200)
```

In this way the rect element acts as a library element. You can edit it but it won't be rendered.

Another way is to point an external SVG file, just specified the element `id` and path to file.

```javascript
var use  = draw.use('elementId', 'path/to/file.svg')
```

This way is usefull when you have complex images already created.
Note that, for external images (outside your domain) it may be necessary to load the file with XHR.

__`returns`: `SVG.Use`__

_Javascript inheritance stack: `SVG.Use` < `SVG.Shape` < `SVG.Element`_

#### Symbol
Not unlike the `group` element, the `symbol` element is a container element. The only difference between symbols and groups is that symbols are not rendered. Therefore a `symbol` element is ideal in combination with the `use` element:

```javascript
var symbol = draw.symbol()
symbol.rect(100, 100).fill('#f09')

var use  = draw.use(symbol).move(200, 200)
```

__`returns`: `SVG.Bare`__

_Javascript inheritance stack: `SVG.Bare` < `SVG.Element` [with a shallow inheritance from `SVG.Parent`]_

#### Bare
For all SVG elements that are not described by SVG.js, the `SVG.Bare` class comes in handy. This class inherits directly from `SVG.Element` and makes it possible to add custom methods in a separate namespace without polluting the main `SVG.Element` namespace. Consider it your personal playground.

##### element()
The `SVG.Bare` class can be instantiated with the `element()` method on any parent element:

```javascript
var element = draw.element('title')
```
The string value passed as the first argument is the node name that should be generated.

Additionally any existing class name can be passed as the second argument to define from which class the element should inherit:

```javascript
var element = draw.element('symbol', SVG.Parent)
```

This gives you as the user a lot of power. But remember, with great power comes great responsibility.

__`returns`: `SVG.Bare`__

##### words()
The `SVG.Bare` instance carries an additional method to add plain text:

```javascript
var element = draw.element('title').words('This is a title.')
//-> <title>This is a title.</title>
```

__`returns`: `itself`__