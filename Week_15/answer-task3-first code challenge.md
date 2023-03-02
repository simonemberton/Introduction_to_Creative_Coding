# Week 15

## Answer - Task 3 - Game Flow - First code challenge


Now **underneath the draw function** let's write a new function that create a new colour (rgb) and new position (x, y) for will be called every time we want to generate a new position and a new colour for our circle:


- Your sketch.js code should like this

```javascript
let x, y;
let radius = 100;
let r,g,b;
let timer = 10;
let interval = 120;

function setup() {
  // put setup code here
  console.log('hello world');
  createCanvas(windowWidth, windowHeight);
  background(220);
  x = random(windowWidth);
  y = random(windowHeight);
  r = random(255);
  g = random(255);
  b = random(255);
}

function draw() {
  fill(0, 0, 0);
  if (frameCount % interval == 0) { // if the frameCount is divisible by the interval, then the interval (in seconds) has passed and we can draw a new circle
    console.log("new circle!");
    background(220); // set background to grey
    newValues();
    ellipse(x, y, radius*2, radius*2); // draw a circle
  }  
}


function newValues() { // reset x, y and rgb values 
  x = random(windowWidth);
  y = random(windowHeight);
  r = random(255);
  g = random(255);
  b = random(255);
}
```
