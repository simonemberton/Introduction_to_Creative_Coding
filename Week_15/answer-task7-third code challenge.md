# Week 15

## Answer - Task 7 - Using images - Third Code Challenge


To replace the circle with an image (like my 8 bit cat) add an images folder and image file (jpg / png).  

Use the ```loadImage('assets/cat.png');``` function to prelaod the image in ```setup()```.    

Use the ```image(img, x-100, y-100);``` to display the image instead of the ellipse. Note the ```-100`` co-ordinates to account for the image being located at the top left of x, y.


- Your sketch.js code should like this

```javascript
let x, y;
let radius = 100;
let r,g,b;
let timer = 10;
let interval = 120;
let score = 0;
let gameOver = false;
let img;

function setup() {
  // put setup code here
  console.log('hello world 2 mar');
  img = loadImage('assets/cat.png');
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
    background(220); // set background to grey
    fill(r, g, b); // create a colour fill with out random rgb values
    newValues();
    //ellipse(x, y, radius*2, radius*2); // draw a circle
    image(img, x-100, y-100);
  }
  else if (frameCount % interval == 0 && gameOver) {
    console.log("gameOver");
    textSize(100);
      textAlign(CENTER, CENTER);
      text("GAME OVER", width/2, height/2);
  }
  // put drawing code here

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
  if (!gameOver) {
    let d = dist(mouseX, mouseY, x, y); // find the distance between where the mouse is when pressed and our x and y values
    if (d < radius) { // is the distance smaller than the radius of the circle?
      //newCircle(); // create a new circle
      console.log("Score!"); // log that we've been successful 
      score++; // log that we've been successful - we'll update this bit later.
      background(220);
    }
  }
  else {
    let d = dist(mouseX, mouseY, windowWidth/2,  windowHeight/2);
    if (d < radius*2) {
      gameOver = false;
      timer = 10;
      score = 0;
      background(220);
    }
  }
}
```
