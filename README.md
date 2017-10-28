# Canvas Clock

[Xiao Shang](https://github.com/ishx/)

Build an analog clock using HTML canvas.

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock.png "Canvas Clock")

Table of contents

- [1 Clock Intro](#1-clock-intro)
    - [1.1 Create the Canvas](#11-create-the-canvas)
    - [1.2 Code Explained](#12-code-explained)
- [2 Clock Face](#2-clock-face)
    - [2.1 Draw a Clock Face](#21-draw-a-clock-face)
    - [2.2 Code Explained](#22-code-explained)
- [3 clock Numbers](#3-clock-numbers)
    - [3.1 Draw Clock Numbers](#31-draw-clock-numbers)
    - [3.2 Example Explained](#32-example-explained)
- [4 Clock Hands](#4-clock-hands)
    - [4.1 Draw Clock Hands](#41-draw-clock-hands)
    - [4.2 Example Explained](#42-example-explained)
- [5 Clock Start](#5-clock-start)
    - [5.1 Start the Clock](#51-start-the-clock)
    - [5.2 Example Explained](#52-example-explained)
- [Contributing](#contributing)
- [License](#license)

## 1 Clock Intro

### 1.1 Create the Canvas

The clock needs an HTML container. Create an HTML canvas:

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock-1.png "Canvas Clock 1")

HTML code:

```
<!DOCTYPE html>
<html>

<style>
canvas {
    -webkit-border-radius: 60px;
    border-radius: 60px;
    background-color: #FF91AF
}
</style>

<body>

<canvas id="canvas" width="400" height="400"></canvas>

<script>
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var radius = canvas.height / 2;
ctx.translate(radius, radius);
radius = radius * 0.90
drawClock();

function drawClock() {
    ctx.arc(0, 0, radius, 0 , 2*Math.PI);
    ctx.fillStyle = "white";
    ctx.fill();
}
</script>

</body>
</html>
```

### 1.2 Code Explained

Add an HTML `<canvas>` element to your page:

```
<canvas id="canvas" width="400" height="400"></canvas>
```

**Note:** Add an CSS `canvas` style to your page:

```
<style>
canvas {
    -webkit-border-radius: 60px;
    border-radius: 60px;
    background-color: #FF91AF
}
</style>
```

Create a canvas object (var canvas) from the HTML canvas element:

```
var canvas = document.getElementById("canvas");
```

Create a 2d drawing object (var ctx) for the canvas object:

```
var ctx = canvas.getContext("2d");
```

Calculate the clock radius, using the height of the canvas:

```
var radius = canvas.height / 2;
```

> Using the canvas height to calculate the clock radius, makes the clock work for all canvas sizes.

Remap the (0,0) position (of the drawing object) to the center of the canvas:

```
ctx.translate(radius, radius);
```

Reduce the clock radius (to 90%) to draw the clock well inside the canvas:

```
radius = radius * 0.90;
```

Create a function to draw the clock:

```
function drawClock() {
    ctx.arc(0, 0, radius, 0 , 2*Math.PI);
    ctx.fillStyle = "white";
    ctx.fill();
}
```
## 2 Clock Face

### 2.1 Draw a Clock Face

The clock needs a clock face. Create a JavaScript function to draw a clock face:

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock-2.png "Canvas Clock 2")

JavaScript:

```
function drawClock() {
    drawFace(ctx, radius);
}

function drawFace(ctx, radius) {
    var grad;

    ctx.beginPath();
    ctx.arc(0, 0, radius, 0, 2*Math.PI);
    ctx.fillStyle = 'white';
    ctx.fill();

    grad = ctx.createRadialGradient(0,0,radius*0.95, 0,0,radius*1.05);
    grad.addColorStop(0, '#00CC99');
    grad.addColorStop(0.5, 'white');
    grad.addColorStop(1, '#00CC99');
    ctx.strokeStyle = grad;
    ctx.lineWidth = radius*0.1;
    ctx.stroke();

    ctx.beginPath();
    ctx.arc(0, 0, radius*0.1, 0, 2*Math.PI);
    ctx.fillStyle = '#00CC99';
    ctx.fill();
}
```

### 2.2 Code Explained

Create a drawFace() function for drawing the clock face:

```
function drawClock() {
    drawFace(ctx, radius);
}

function drawFace(ctx, radius) {
}
```

Draw the white circle:

```
ctx.beginPath();
ctx.arc(0, 0, radius, 0, 2*Math.PI);
ctx.fillStyle = 'white';
ctx.fill();
```

Create a radial gradient (95% and 105% of original clock radius):

```
grad = ctx.createRadialGradient(0,0,radius*0.95, 0,0,radius*1.05);
```

Create 3 color stops, corresponding with the inner, middle, and outer edge of the arc:

```
grad.addColorStop(0, '#00CC99');
grad.addColorStop(0.5, 'white');
grad.addColorStop(1, '#00CC99');
```

> The color stops create a 3D effect.

Define the gradient as the stroke style of the drawing object:

```
ctx.strokeStyle = grad;
```

Define the line width of the drawing object (10% of radius):

```
ctx.lineWidth = radius * 0.1;
```

Draw the circle:
```
ctx.stroke();
```

Draw the clock center:

```
ctx.beginPath();
ctx.arc(0, 0, radius*0.1, 0, 2*Math.PI);
ctx.fillStyle = '#00CC99';
ctx.fill();
```

## 3 clock Numbers

### 3.1 Draw Clock Numbers

The clock needs numbers. Create a JavaScript function to draw clock numbers:

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock-3.png "Canvas Clock 3")

JavaScript:

```
function drawClock() {
    drawFace(ctx, radius);
    drawNumbers(ctx, radius);
}

function drawNumbers(ctx, radius) {
    var ang;
    var num;
    ctx.font = radius*0.15 + "px arial";
    ctx.textBaseline="middle";
    ctx.textAlign="center";
    for(num= 1; num < 13; num++){
        ang = num * Math.PI / 6;
        ctx.rotate(ang);
        ctx.translate(0, -radius*0.85);
        ctx.rotate(-ang);
        ctx.fillText(num.toString(), 0, 0);
        ctx.rotate(ang);
        ctx.translate(0, radius*0.85);
        ctx.rotate(-ang);
    }
}
```

### 3.2 Example Explained

Set the font size (of the drawing object) to 15% of the radius:

```
ctx.font = radius*0.15 + "px arial";
```

Set the text alignment to the middle and the center of the print position:

```
ctx.textBaseline="middle";
ctx.textAlign="center";
```

Calculate the print position (for 12 numbers) to 85% of the radius, rotated (PI/6) for each number:

```
for(num= 1; num < 13; num++) {
    ang = num * Math.PI / 6;
    ctx.rotate(ang);
    ctx.translate(0, -radius*0.85);
    ctx.rotate(-ang);
    ctx.fillText(num.toString(), 0, 0);
    ctx.rotate(ang);
    ctx.translate(0, radius*0.85);
    ctx.rotate(-ang); 
}
```

## 4 Clock Hands

### 4.1 Draw Clock Hands

The clock needs hands. Create a JavaScript function to draw clock hands:

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock-4.png "Canvas Clock 4")

JavaScript:

```
function drawClock() {
    drawFace(ctx, radius);
    drawNumbers(ctx, radius);
    drawTime(ctx, radius);
}

function drawTime(ctx, radius){
    var now = new Date();
    var hour = now.getHours();
    var minute = now.getMinutes();
    var second = now.getSeconds();
    //hour
    hour=hour%12;
    hour=(hour*Math.PI/6)+(minute*Math.PI/(6*60))+(second*Math.PI/(360*60));
    drawHand(ctx, hour, radius*0.5, radius*0.07);
    //minute
    minute=(minute*Math.PI/30)+(second*Math.PI/(30*60));
    drawHand(ctx, minute, radius*0.8, radius*0.07);
    // second
    second=(second*Math.PI/30);
    ctx.strokeStyle = 'red';
    drawHand(ctx, second, radius*0.9, radius*0.02);
}

function drawHand(ctx, pos, length, width) {
    ctx.beginPath();
    ctx.lineWidth = width;
    ctx.lineCap = "round";
    ctx.moveTo(0,0);
    ctx.rotate(pos);
    ctx.lineTo(0, -length);
    ctx.stroke();
    ctx.rotate(-pos);
}
```

### 4.2 Example Explained

Use Date to get hour, minute, second:

```
var now = new Date();
var hour = now.getHours();
var minute = now.getMinutes();
var second = now.getSeconds();
```

Calculate the angle of the hour hand, and draw it a length (50% of radius), and a width (7% of radius):

```
hour=hour%12;
hour=(hour*Math.PI/6)+(minute*Math.PI/(6*60))+(second*Math.PI/(360*60));
drawHand(ctx, hour, radius*0.5, radius*0.07);
```
Use the same technique for minutes and seconds.

The drawHand() routine does not need an explanation. It just draws a line with a given length and width.

## 5 Clock Start

### 5.1 Start the Clock

To start the clock, call the drawClock function at intervals:

![alt text](https://github.com/ishx/canvasclock/blob/master/assets/img/canvasclock-5.png "Canvas Clock 5")

JavaScript:

```
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var radius = canvas.height / 2;
ctx.translate(radius, radius);
radius = radius * 0.90
//drawClock();
setInterval(drawClock, 1000);
```

### 5.2 Example Explained

The only thing you have to do (to start the clock) is to call the drawClock function at intervals.

Substitute:

```
drawClock();
```

With:

```
setInterval(drawClock, 1000);
```

> The interval is in milliseconds. drawClock will be called for each 1000 milliseconds.

## Contributing

You are most welcome to contribute to canvasclock development by [forking this repository on GitHub](https://github.com/ishx/canvasclock) and sending pull requests, or filing bug reports at the [issues page](http://github.com/ishx/canvasclock/issues). If it is a big feature, you might want to start an Issue first to make sure it's something that will be accepted.  If it involves code, please also write tests for it.

## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc/3.0/">
    <img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-nc/3.0/88x31.png" />
</a>

The source code for the site is licensed under the MIT license, which you can find in the [LICENSE](https://github.com/ishx/canvasclock/blob/master/LICENSE) file.

All graphical assets are licensed under the [Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/).