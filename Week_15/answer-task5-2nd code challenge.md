# Week 15

## Answer - Task 5 - sceond Code Challenge

See the solution for the final code to get ```gameOver`` working.   

- Your sketch.js code should like this

```javascript
let x, y;
let radius = 100;
let r,g,b;
let timer = 10;
let interval = 120;
let score = 0;
let gameOver = false;

function setup() {
  // put setup code here
  console.log('hello world 2 mar');
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
  textSize(24);
  textAlign(LEFT, CENTER);
  text("Score: " + score, 10, 30);
  text("Countdown: " + timer, 120, 30);
  if (frameCount % interval == 0 && !gameOver) { // if the frameCount is divisible by the interval, then the interval (in seconds) has passed and we can draw a new circle
    timer --;
    console.log("new circle!");
    background(220); // set background to grey
    newValues();
    ellipse(x, y, radius*2, radius*2); // draw a circle
  }
  else if (frameCount % interval == 0 && gameOver) {
    console.log("gameOver");
    textSize(100);
      textAlign(CENTER, CENTER);
      text("GAME OVER", width/2, height/2);
  }

  // decide when gameOver
  if (timer < 1) {
    gameOver = true;
  } 
}


function newValues() { // reset x, y and rgb values 
  x = random(windowWidth);
  y = random(windowHeight);
  r = random(255);
  g = random(255);
  b = random(255);
}

function mousePressed() {
  let d = dist(mouseX, mouseY, x, y); // find the distance between where the mouse is when pressed and our x and y values
  if (d < radius) { // is the distance smaller than the radius of the circe?
    newValues(); // create a new circle
    console.log("Score!", score); // log that we've been successful 
    score++; // log that we've been successful - we'll update this bit later.
  }
}




```



