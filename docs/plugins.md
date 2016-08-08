# Extend svg.js / Plugins

## Extending functionality

### SVG.invent()
Creating your own custom elements with SVG.js is a piece of cake thanks to the `SVG.invent` function. For the sake of this example, lets "invent" a shape. We want a `rect` with rounded corners that are always proportional to the height of the element. The new shape lives in the `SVG` namespace and is called `Rounded`. Here is how we achieve that.

```javascript
SVG.Rounded = SVG.invent({
  // Define the type of element that should be created
  create: 'rect'

  // Specify from which existing class this shape inherits
, inherit: SVG.Shape

  // Add custom methods to invented shape
, extend: {
    // Create method to proportionally scale the rounded corners
    size: function(width, height) {
      return this.attr({
        width:  width
      , height: height
      , rx:     height / 5
      , ry:     height / 5
      })
    }
  }

  // Add method to parent elements
, construct: {
    // Create a rounded element
    rounded: function(width, height) {
      return this.put(new SVG.Rounded).size(width, height)
    }

  }
})
```

To create the element in your drawing:

```javascript
var rounded = draw.rounded(200, 100)
```

That's it, the invention is now ready to be used!

#### Accepted values
The `SVG.invent()` function always expects an object. The object can have the following configuration values:

- `create`: can be either a string with the node name (e.g. `rect`, `ellipse`, ...) or a custom initializer function; `[required]`
- `inherit`: the desired SVG.js class to inherit from (e.g. `SVG.Shape`, `SVG.Element`, `SVG.Container`, `SVG.Rect`, ...); `[optional but recommended]`
- `extend`: an object with the methods that should be applied to the element's prototype; `[optional]`
- `construct`: an object with the methods to create the element on the parent element; `[optional]`
- `parent`: an SVG.js parent class on which the methods in the passed `construct` object should be available - defaults to `SVG.Container`; `[optional]`

#### Usage notes

It should be emphasized that in the configuration object passed to `SVG.invent()`:

- `construct` does not supply constructors, but rather methods that are likely to *call* constructors;
- `create` specifies the constructor for the type you are defining, and is not analogous to `Object.create()`.

When defining specialized svg elements (such as `SVG.Rounded` in the example above), the function specified by `create` needs to do all the work of adding the element to the DOM for the svg document and connecting the DOM node to the SVG.js interface. All this is done automatically when the value of `create` is a string identifying an element type. If needed, see the source for a sense of how to do it explicitly.

Though the defaults are geared toward creating svg elements for the SVG.js framework, `SVG.invent()` can be used as a generalized function for defining types in Javascript. When used in this more general way, the function supplied as a value for `create` should be written as an ordinary JS constructor. (Indeed, the function is simply returned as the constructor for your newly defined type.)

Svg.js uses the `SVG.invent()` function to create all internal elements. A look at the source will show how this function is used in various ways.



### SVG.extend()
SVG.js has a modular structure. It is very easy to add your own methods at different levels. Let's say we want to add a method to all shape types then we would add our method to SVG.Shape:

```javascript
SVG.extend(SVG.Shape, {
  paintRed: function() {
    return this.fill('red')
  }
})
```

Now all shapes will have the paintRed method available. Say we want to have the paintRed method on an ellipse apply a slightly different color:

```javascript
SVG.extend(SVG.Ellipse, {
  paintRed: function() {
    return this.fill('orangered')
  }
})

```
The complete inheritance stack for `SVG.Ellipse` is:

_`SVG.Ellipse` < `SVG.Shape` < `SVG.Element`_

The SVG document can be extended by using:

```javascript
SVG.extend(SVG.Doc, {
  paintAllPink: function() {
    this.each(function() {
      this.fill('pink')
    })
  }
})
```

You can also extend multiple elements at once:
```javascript
SVG.extend(SVG.Ellipse, SVG.Path, SVG.Polygon, {
  paintRed: function() {
    return this.fill('orangered')
  }
})
```


## Plugins
Here are a few nice plugins that are available for SVG.js:

### pathmorphing
[svg.pathmorphing.js](https://github.com/wout/svg.pathmorphing.js) to make path animatable

### textmorphing
[svg.textmorph.js](https://github.com/wout/svg.textmorph.js) to make text animatable

### absorb
[svg.absorb.js](https://github.com/wout/svg.absorb.js) absorb raw SVG data into an SVG.js instance.

### draggable
[svg.draggable.js](https://github.com/wout/svg.draggable.js) to make elements draggable.

### connectable
[svg.connectable.js](https://github.com/jillix/svg.connectable.js) to connect elements.

[svg.connectable.js fork](https://github.com/loredanacirstea/svg.connectable.js) to connect elements (added: curved connectors, you can use any self-made path as a connector, choosable 'center'/'perifery' attachment, 'perifery' attachment for source / target SVG Paths uses smallest-distance algorithm between PathArray points)

### easing
[svg.easing.js](https://github.com/wout/svg.easing.js) for more easing methods on animations.

### export
[svg.export.js](https://github.com/wout/svg.export.js) export raw SVG.

### filter
[svg.filter.js](https://github.com/wout/svg.filter.js) adding svg filters to elements.

### foreignobject
[svg.foreignobject.js](https://github.com/john-memloom/svg.foreignobject.js) foreignObject implementation (by john-memloom).

### import
[svg.import.js](https://github.com/wout/svg.import.js) import raw SVG data.

### math
[svg.math.js](https://github.com/otm/svg.math.js) a math extension (by Nils Lagerkvist).

### path
[svg.path.js](https://github.com/otm/svg.path.js) for manually drawing paths (by Nils Lagerkvist).

### screenBBox
[svg.screenbbox.js](https://github.com/fuzzyma/svg.screenbbox.js) to get the bbox in screen coordinates from transformed path/polygon/polyline

### shapes
[svg.shapes.js](https://github.com/wout/svg.shapes.js) for more polygon based shapes.

### topath
[svg.topath.js](https://github.com/wout/svg.topath.js) to convert any other shape to a path.

### topoly
[svg.topoly.js](https://github.com/wout/svg.topoly.js) to convert a path to polygon or polyline.

### wiml
[svg.wiml.js](https://github.com/wout/svg.wiml.js) a templating language for svg output.

### comic
[comic.js](https://github.com/balint42/comic.js) to cartoonize any given svg.

### draw
[svg.draw.js](https://github.com/fuzzyma/svg.draw.js) to draw elements with your mouse

### select
[svg.select.js](https://github.com/fuzzyma/svg.select.js) to select elements

### resize
[svg.resize.js](https://github.com/fuzzyma/svg.resize.js) to resize elements with your mouse