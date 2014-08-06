# colorPicker

An advanced, fast but small **javaScript (only) color picker** (color chooser for desktop use) that uses only one .js file (no extra HTML, CSS, images, etc... on IE9+ and internet browsers), works though in all browsers incl. IE6+ (an extra CSS has to be made to make it work in IE6 though and some additional setTimeout tricks to make it work in IE5.5)<br />
**colorPicker.js** uses **colors.js**, a small but comprehensive tool for color conversions / calculations... 

With **colors.js** (~8.7k or way smaller if other color spaces and complex calculations are taken out...) you can also build your own simpler and / or smaller color pickers quite easy as demonstrated on the demo page.<br />
**colors.js** can convert 182 different combinations of color spaces (rgb2HSV, RGB2hsl, rgb2RGB, HEX2Lab, ...)

##Demo
See **demo** at http://dematte.at/cpn

<img src="images/screen-shot-all.png" />

All the W3C 2.0 calculations for readability are also based on opacity levels of all layers<br>
Supported color spaces are: (* also displayed as colors in realtime)<br>
rgb *, hsv(b) *, hsl *, cmyk, cmy, Lab, XYZ, HEX

##colors.js

```javascript
var myColors = new Colors({ // all options have a default value...
    color: 'rgba(204, 82, 37, 0.8)', // initial color (#RGB, RGB, #RRGGBB, RRGGBB, rgb(r, g, b), ...)
    XYZMatrix: ..., // Observer = 2° (CIE 1931), Illuminant = D65  --- see source for dedtails
    XYZReference: ..., // back reference to XYZMatrix --- see source for dedtails
    grey: {r: 0.298954, g: 0.586434, b: 0.114612}, // CIE-XYZ 1931
    luminance: {r: 0.2126, g: 0.7152, b: 0.0722}, // W3C 2.0
    valueRanges: {rgb: {r: [0, 255], g: [0, 255], b: [0, 255]}, hsv:...}, // skip ranges if no conversion required
    customBG: '#808080' // the solid bgColor behind the chosen bgColor (saved color)
    convertCallback: function(colors, type){}, // callback function after color convertion for further calculations...
    allMixDetails: false // if set to true, Colors deliveres some more mixed layer informations for all color layers
});
```
##colorPicker.js

colorPicker uses an instance of Colors and passes the options to it, so some values are the same...

```javascript
var myColorPicker = new ColorPicker({
    color: ..., // see Colors...
    mode: 'rgb-b', // initial mode the color picker is starting with
    fps: 60, // the framerate colorPicker refreshes the display if no 'requestAnimationFrame'
    delayOffset: 8, // pixels offset when shifting mouse up/down inside input fields before it starts acting as slider
    CSSPrefix: 'cp-', // the standard prefix for (almost) all class declarations (HTML, CSS)
    size: 0, // one of the 4 sizes: 0 = L (large), 1 = S, 2 = XS, 3 = XXS; resize to see what happens...
    allMixDetails: true, // see Colors...
    alphaBG: 'w' // initial 3rd layer bgColor (w = white, c = custom (customBG), b = black);
    customBG: '#808080' // see Colors...
    noAlpha: true, // disable alpha input (all sliders are gone and current alpha therefore locked)
    cmyOnly: false, // display CMY instead of CMYK
    memoryColors: [{r: 100, g: 200, b: 10, a: 0.8}, ...] // array of colors in memory section
    opacityPositionRelative: undefined, // render opacity slider arrows in px or %
    customCSS: undefined, // if external stylesheet, internal will be ignored...
    appenTo: document.body, // the HTMLElement the colorPicker will be appended to on initialization
    noRangeBackground: false, // performance option: doesn't render backgrounds in input fields if set to false
    textRight: false, // not supported yet. Make numbers appear aligned right
    noHexButton: false, // button next to HEX input could be used for some trigger...
    noResize: false, // enable / disable resizing of colorPicker
    noRGBr: false, // active / passive button right to RGB-R display. Disables rendering of 'real' color possibilities...
    noRGBg: false, // same as above
    noRGBb: false, // same as above
    CSSStrength: 'div.', // not in use
    XYZMatrix: ..., // see Colors...
    XYZReference: ..., // see Colors...
    grey: ..., // see Colors...
    luminance: ..., // see Colors...
    devPicker: false // uses existing HTML instead of internal template for developing
    renderCallback: // function(colors, mode){}, // callback on after rendering (for further rendering outside colorPicker)
    actionCallback: function(e, action){}, // callback on any action within colorPicker (buttons, sliders, ...)
    convertCallback: function(colors, type){}, // see Colors...
});
```

##The color model, the methods and more

After initializing Color or ColorPicker you'll get a clean but rhich model of the instance:

```javascript
myColors: {
    colors: { all kinds of color values... },
    options: { all the options you set or that are set as default... },
    __proto__: { // all methods Color uses
        setColor: function(newCol, type, alpha) {},
        getColor: function(type) {},
        setCustomBackground: function(col) {},
        saveAsBackground: function() {},
        convertColor: function(color, type) {} // converts 182 different combinations
    }
}
```

```javascript
myColorPicker: {
    color: { // instance of Color inside colorPicker
        colors: { all kinds of color values... },
        options: { all the options you set or that are set as default... },
        __proto__: { all methods Color uses ... }
    },
    nodes: { all kinds of cashed nodes, textNodes and styles, etc... },
    __proto__: { // all methods ColorPicker uses
        setColor: function(newCol, type, alpha, forceRender) {},
        startRender: function(oneTime) {},
        stopRender: function() {},
        setCustomBackground: function(col) {},
        saveAsBackground: function() {},
        setMode: function(mode) {},
        destroyAll: function() {}
    }
}
```

The klass ColorPicker has some functions attached, used inside, exported for convenience...

```javascript
ColorPicker.addEvent = function(obj, type, func){}; // with built in cashe
ColorPicker.removeEvent = function(obj, type, func){};
ColorPicker.getOrigin = function(elm){};
ColorPicker.limitValue = function(value, min, max){};
ColorPicker.changeClass = function(elm, cln, newCln){}; // a bit tricky to use...
```
