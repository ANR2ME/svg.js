### Parent elements

#### Main svg document
The main SVG.js initializer function creates a root svg node in the given element and returns an instance of `SVG.Doc`:

```javascript
var draw = SVG('drawing')
```

__`returns`: `SVG.Doc`__

_Javascript inheritance stack: `SVG.Doc` < `SVG.Container` < `SVG.Parent`_

#### Nested svg
With this feature you can nest svg documents within each other. Nested svg documents have exactly the same features as the main, top-level svg document:

```javascript
var nested = draw.nested()

var rect = nested.rect(200, 200)
```

__`returns`: `SVG.Nested`__

_Javascript inheritance stack: `SVG.Nested` < `SVG.Container` < `SVG.Parent`_

#### Groups
Grouping elements is useful if you want to transform a set of elements as if it were one. All element within a group maintain their position relative to the group they belong to. A group has all the same element methods as the root svg document:

```javascript
var group = draw.group()
group.path('M10,20L30,40')
```

Existing elements from the svg document can also be added to a group:

```javascript
group.add(rect)
```

__Note:__ Groups do not have a geometry of their own, it's inherited from their content. Therefore groups do not listen to `x`, `y`, `width` and `height` attributes. If that is what you are looking for, use a `nested()` svg instead.

__`returns`: `SVG.G`__

_Javascript inheritance stack: `SVG.G` < `SVG.Container` < `SVG.Parent`_

#### Hyperlink
A hyperlink or `<a>` tag creates a container that enables a link on all children:

```javascript
var link = draw.link('http://svgjs.com')
var rect = link.rect(100, 100)
```

The link url can be updated with the `to()` method:

```javascript
link.to('http://apple.com')
```

Furthermore, the link element has a `show()` method to create the `xlink:show` attribute:

```javascript
link.show('replace')
```

And the `target()` method to create the `target` attribute:

```javascript
link.target('_blank')
```

Elements can also be linked the other way around with the `linkTo()` method:

```javascript
rect.linkTo('http://svgjs.com')
```

Alternatively a block can be passed instead of a url for more options on the link element:

```javascript
rect.linkTo(function(link) {
  link.to('http://svgjs.com').target('_blank')
})
```

__`returns`: `SVG.A`__

_Javascript inheritance stack: `SVG.A` < `SVG.Container` < `SVG.Parent`_

#### Defs
The `<defs>` element is a container element for referenced elements. Elements that are descendants of a ‘defs’ are not rendered directly. The `<defs>` node lives in the main `<svg>` document and can be accessed with the `defs()` method:

```javascript
var defs = draw.defs()
```

The defs are also available on any other element through the `doc()` method:

```javascript
var defs = rect.doc().defs()
```

The defs node works exactly the same as groups.

__`returns`: `SVG.Defs`__

_Javascript inheritance stack: `SVG.Defs` < `SVG.Container` < `SVG.Parent`_