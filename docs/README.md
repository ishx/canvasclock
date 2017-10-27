---
title: Canvas Clock
---
# Canvas Clock

[Xiao Shang](http://github.com/ishx)

Build an analog clock using HTML canvas.

<style>
canvas {
    -webkit-border-radius: 60px;
    border-radius: 60px;
    background-color: #FF91AF
}
</style>

<canvas id="canvas" width="400" height="400"></canvas>

<script>
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var radius = canvas.height / 2;
ctx.translate(radius, radius);
radius = radius * 0.90
setInterval(drawClock, 1000);

function drawClock() {
  drawFace(ctx, radius);
  drawNumbers(ctx, radius);
  drawTime(ctx, radius);
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

function drawNumbers(ctx, radius) {
  var ang;
  var num;
  ctx.font = radius*0.15 + "px arial";
  ctx.textBaseline="middle";
  ctx.textAlign="center";
  for(num = 1; num < 13; num++){
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

function drawTime(ctx, radius){
    var now = new Date();
    var hour = now.getHours();
    var minute = now.getMinutes();
    var second = now.getSeconds();
    //hour
    hour=hour%12;
    hour=(hour*Math.PI/6)+
    (minute*Math.PI/(6*60))+
    (second*Math.PI/(360*60));
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
</script>

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


## 1 Clock Intro

### 1.1 Create the Canvas

The clock needs an HTML container. Create an HTML canvas:

<canvas id="canvas1" width="400" height="400"></canvas>

<script>
var canvas1 = document.getElementById("canvas1");
var ctx1 = canvas1.getContext("2d");
var radius1 = canvas1.height / 2;
ctx1.translate(radius1, radius1);
radius1 = radius1 * 0.90
drawClock1();

function drawClock1() {
    ctx1.arc(0, 0, radius1, 0 , 2*Math.PI);
    ctx1.fillStyle = "white";
    ctx1.fill();
}
</script>


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

<canvas id="canvas2" width="400" height="400"></canvas>

<script>
var canvas2 = document.getElementById("canvas2");
var ctx2 = canvas2.getContext("2d");
var radius2 = canvas2.height / 2;
ctx2.translate(radius2, radius2);
radius2 = radius2 * 0.90
drawClock2();

function drawClock2() {
  drawFace2(ctx2, radius2);
}

function drawFace2(ctx, radius) {
  var grad;
  ctx.beginPath();
  ctx.arc(0, 0, radius, 0, 2*Math.PI);
  ctx.fillStyle = 'white';
  ctx.fill();
  grad = ctx2.createRadialGradient(0,0,radius*0.95, 0,0,radius*1.05);
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
</script>

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

<canvas id="canvas3" width="400" height="400"></canvas>

<script>
var canvas3 = document.getElementById("canvas3");
var ctx3 = canvas3.getContext("2d");
var radius3 = canvas3.height / 2;
ctx3.translate(radius3, radius3);
radius3 = radius3 * 0.90
drawClock3();

function drawClock3() {
  drawFace3(ctx3, radius3);
  drawNumbers3(ctx3, radius3);
}

function drawFace3(ctx, radius) {
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

function drawNumbers3(ctx, radius) {
  var ang;
  var num;
  ctx.font = radius*0.15 + "px arial";
  ctx.textBaseline="middle";
  ctx.textAlign="center";
  for(num = 1; num < 13; num++){
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
</script>

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

<canvas id="canvas4" width="400" height="400"></canvas>

<script>
var canvas4 = document.getElementById("canvas4");
var ctx4 = canvas4.getContext("2d");
var radius4 = canvas4.height / 2;
ctx4.translate(radius4, radius4);
radius4 = radius4 * 0.90
drawClock4();

function drawClock4() {
  drawFace4(ctx4, radius4);
  drawNumbers4(ctx4, radius4);
  drawTime4(ctx4, radius4);
}

function drawFace4(ctx, radius) {
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

function drawNumbers4(ctx, radius) {
  var ang;
  var num;
  ctx.font = radius*0.15 + "px arial";
  ctx.textBaseline="middle";
  ctx.textAlign="center";
  for(num = 1; num < 13; num++){
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

function drawTime4(ctx, radius){
    var now = new Date();
    var hour = now.getHours();
    var minute = now.getMinutes();
    var second = now.getSeconds();
    //hour
    hour=hour%12;
    hour=(hour*Math.PI/6)+
    (minute*Math.PI/(6*60))+
    (second*Math.PI/(360*60));
    drawHand4(ctx, hour, radius*0.5, radius*0.07);
    //minute
    minute=(minute*Math.PI/30)+(second*Math.PI/(30*60));
    drawHand4(ctx, minute, radius*0.8, radius*0.07);
    // second
    second=(second*Math.PI/30);
    ctx.strokeStyle = 'red';
    drawHand4(ctx, second, radius*0.9, radius*0.02);
}

function drawHand4(ctx, pos, length, width) {
    ctx.beginPath();
    ctx.lineWidth = width;
    ctx.lineCap = "round";
    ctx.moveTo(0,0);
    ctx.rotate(pos);
    ctx.lineTo(0, -length);
    ctx.stroke();
    ctx.rotate(-pos);
}
</script>

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

<canvas id="canvas5" width="400" height="400"></canvas>

<script>
var canvas5 = document.getElementById("canvas5");
var ctx5 = canvas5.getContext("2d");
var radius5 = canvas5.height / 2;
ctx5.translate(radius5, radius5);
radius5 = radius5 * 0.90
setInterval(drawClock5, 1000);

function drawClock5() {
  drawFace(ctx5, radius5);
  drawNumbers(ctx5, radius5);
  drawTime(ctx5, radius5);
}

function drawFace5(ctx, radius) {
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

function drawNumbers5(ctx, radius) {
  var ang;
  var num;
  ctx.font = radius*0.15 + "px arial";
  ctx.textBaseline="middle";
  ctx.textAlign="center";
  for(num = 1; num < 13; num++){
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

function drawTime5(ctx, radius){
    var now = new Date();
    var hour = now.getHours();
    var minute = now.getMinutes();
    var second = now.getSeconds();
    //hour
    hour=hour%12;
    hour=(hour*Math.PI/6)+
    (minute*Math.PI/(6*60))+
    (second*Math.PI/(360*60));
    drawHand5(ctx, hour, radius*0.5, radius*0.07);
    //minute
    minute=(minute*Math.PI/30)+(second*Math.PI/(30*60));
    drawHand5(ctx, minute, radius*0.8, radius*0.07);
    // second
    second=(second*Math.PI/30);
    ctx.strokeStyle = 'red';
    drawHand5(ctx, second, radius*0.9, radius*0.02);
}

function drawHand5(ctx, pos, length, width) {
    ctx.beginPath();
    ctx.lineWidth = width;
    ctx.lineCap = "round";
    ctx.moveTo(0,0);
    ctx.rotate(pos);
    ctx.lineTo(0, -length);
    ctx.stroke();
    ctx.rotate(-pos);
}
</script>

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