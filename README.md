## Building Juicy Minimal Roguelikes in the Browser

A talk by [Chris McCormick](https://mccormick.cx) ([@mccrmx](https://twitter.com/mccrmx)) for Roguelike Celebration 2021.

[Get the slides](./building-juicy-minimal-roguelikes-in-the-browser.odp).

## Resources

 * [Slingcode.net online web app editor](https://slingcode.net)

### ROT.js

 * [ROT.js Roguelike Toolkit in JavaScript](http://ondras.github.io/rot.js/hp/)

Use it in your HTML like this:

```html
<script src="https://cdn.jsdelivr.net/npm/rot-js@2/dist/rot.js"></script>
```

 * [ROT.js tutorial on RogueBasin](http://roguebasin.com/?title=Rot.js_tutorial)
 * [ROT.js tutorial result](https://jsfiddle.net/rotjs/qRnFY/)
 * [ROT.js tilemap example](http://jsfiddle.net/vqy1Lgnx/1/)

### The working demo code from my presentation

 * [index.html](./index.html) (Micro-rogue tilemap.png is by [Kenney.nl](https://kenney.nl/)).

### Further documentation

 * [ROT.js page on RogueBasin](http://roguebasin.com/?title=Rot.js)
 * [ROT.js interactive manual](https://ondras.github.io/rot.js/manual/)

### My games etc.

 * [Asterogue](https://asterogue.space) (Android and Windows)
 * [Smallest Quest](https://thepunkcollective.itch.io/smallest-quest) (Windows, Mac, Linux)
 * [Roguelike Browser Boilerplate](https://chr15m.itch.io/roguelike-browser-boilerplate)
 * [The Punk Collective](https://thepunkcollective.com/)

## Code modifications

Here are the changes I made during the talk to turn Ondřej's jsfiddle demos into a standalone web app in [index.html](./index.html).

Adding the script tag:

```html
<script src="https://cdn.jsdelivr.net/npm/rot-js@2/dist/rot.js"></script>
```

Added a border to the canvas:

```css
canvas { border: 1px solid silver; }
```

Implementing a tile set:

```javascript

var tileSet = document.createElement("img");
tileSet.src = "tilemap.png";

var options = {
    layout: "tile",
    bg: "transparent",
    tileWidth: 40,
    tileHeight: 40,
    tileSet: tileSet,
    tileMap: {
        "@": [4, 0],
        ".": [4, 4],
        "P": [11, 0],
        "*": [9, 3],
    },
    width: 18,
    height: 20,
}

Object.keys(options.tileMap).forEach(function(k) {
  options.tileMap[k][0] *= options.tileWidth;
  options.tileMap[k][1] *= options.tileHeight;
})


this.display = new ROT.Display(options);


var digger = new ROT.Map.Digger(options.width, options.height);


tileSet.onload = function() {
	Game.init();
}
```

Adding an overlayed text with a font from Google Web Fonts:

```html
<p>Hit points: 5</p>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap" rel="stylesheet">
```

The CSS to get the fonts working:

```css
body { font-family: 'Press Start 2P', cursive; text-align: center; }

p { position: absolute; top: 36px; margin: auto; text-align: center; width: 100%; display: block; }
```

Adding an overlaid box:

```html
<div id="box"></div>
```

```css
div {
  position: absolute; top: 100px; left: 100px; border: 1px solid red;
}
```

Inside the Player's draw method to make the box follow the player:

```javascript
    s = box.style;
    s.width = options.tileWidth + "px";
    s.height = options.tileHeight + "px";
    c = document.querySelector("canvas");
    cleft = (c.offsetLeft - c.scrollLeft + c.clientLeft);
    ctop = (c.offsetTop - c.scrollTop + c.clientTop);
    s.left = (this._x * options.tileWidth + cleft) + "px";
    s.top = (this._y * options.tileHeight + ctop) + "px";
```

Adding a basic onclick handler to the box:

```javascript
box.onclick = function(ev) {
  alert(ev.clientX + ", " + ev.clientY);
}
```
