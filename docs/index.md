# SVG.js

A lightweight library for manipulating and animating SVG.

Svg.js has no dependencies and aims to be as small as possible.

Svg.js is licensed under the terms of the MIT License.

Want to know more? Check the [documentation](http://documentup.com/wout/SVG.js).

[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=pay%40woutfierens.com&lc=US&item_name=SVG.JS&currency_code=EUR&bn=PP-DonationsBF%3Abtn_donate_74x21.png%3ANonHostedGuest)

## Usage

### Create an SVG document

Use the `SVG()` function to create an SVG document within a given html element:

```javascript
var draw = SVG('drawing').size(300, 300)
var rect = draw.rect(100, 100).attr({ fill: '#f06' })
```
The first argument can either be an id of the element or the selected element itself.
This will generate the following output:

```html
<div id="drawing">
  <svg xmlns="http://www.w3.org/2000/svg" version="1.1" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" height="300">
    <rect width="100" height="100" fill="#f06"></rect>
  </svg>
</div>
```

By default the svg drawing follows the dimensions of its parent, in this case `#drawing`:

```javascript
var draw = SVG('drawing').size('100%', '100%')
```

### Checking for SVG support

By default this library assumes the client's browser supports SVG. You can test support as follows:

```javascript
if (SVG.supported) {
  var draw = SVG('drawing')
  var rect = draw.rect(100, 100)
} else {
  alert('SVG not supported')
}
```


### SVG document
Svg.js also works outside of the HTML DOM, inside an SVG document for example:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<svg id="drawing" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" >
  <script type="text/javascript" xlink:href="svg.min.js"></script>
  <script type="text/javascript">
    <![CDATA[
      var draw = SVG('drawing')
      draw.rect(100,100).animate().fill('#f03').move(100,100)
    ]]>
  </script>
</svg>
```

### Sub-pixel offset fix
Call the `spof()` method to fix sub-pixel offset:

```javascript
var draw = SVG('drawing').spof()
```

To enable automatic sub-pixel offset correction when the window is resized:

```javascript
SVG.on(window, 'resize', function() { draw.spof() })
```