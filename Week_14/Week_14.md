# Week 14

## Simple Game Exercise

This exercise leans heavily on [this tutorial](https://kellylougheed.medium.com/make-your-first-game-with-p5-js-38bfb308a671) by Kelly Lougheed. So credit goes to her for the idea and most of the implementation!

### Task 1 - Timing and Drawing a Random Circle

- Global variables
```javascript
let x, y;
let radius = 100;
let r,g,b;
let timer = 10;
let interval = 60;
```

- setup function

```javascript
function setup() {
  createCanvas(windowWidth, windowHeight);
  x = random(windowWidth);
  y = random(windowHeight);
  r = random(255);
  g = random(255);
  b = random(255);
}
```
- new circle function under draw()
```javascript
function newCircle() { // reset x, y and rgb values 
  x = random(windowWidth);
  y = random(windowHeight);
  r = random(255);
  g = random(255);
  b = random(255);
}
```
- Draw functon using an inter

```javascript
function draw() {
  background(220); // set background to grey
  noStroke(); // turn stroke off
    fill(r, g, b); // create a colour fill with out random rgb values
    ellipse(x, y, radius*2, radius*2); // draw a circle
	  if (frameCount % interval == 0) { // if the frameCount is divisible by the interval, then the interval (in seconds) has passed and we can draw a new circle
	      
	      newCircle();
	    }
}
```

### Task 2 - Mouse Interaction / Collision Detection

- 
```javascript
function mousePressed() {

let d = dist(mouseX, mouseY, x, y); // find the distance between where the mouse is when pressed and our x and y values
       if (d < radius) { // is the distance smaller than the radius of the circe?
          newCircle(); // create a new circle
          console.log("Score!"); // log that we've been successful - we'll update this bit later.
        }
}
```

### Task 3 - Keeping Score

```javascript
let score = 0;
```


- Show score on screen
```javascript
textSize(24);
textAlign(LEFT, CENTER);
text("Score: " + score, 10, 30);
```

- Increment score

```javascript
if (d < radius) { // is the distance smaller than the radius of the circe?
          newCircle(); // create a new circle
          score++; // log that we've been successful - we'll update this bit later.
      }
```

### Task 4 - Countdown Timer

```javascript
let gameOver = false;
```
- 
```javascript
text("Countdown: " + timer, 120, 30);
```

-

```javascript
 if (frameCount % interval == 0 && timer > 0) { // if the frameCount is divisible by the interval, then the interval (in seconds) has passed and we can decrement timer and draw a new circle
	      timer --;
	      newCircle();
	}

```

### Task 5 - Game Flow

```javascript
let gameOver = false;
```
-
<details>
<summary>Want to see the final code?</summary>
<br>
This is how you dropdown.
<br><br>
<pre>
<code>
function draw() {
  background(220);
  // Draw a circle
  if(!gameOver) {
    noStroke();
    fill(r, g, b);
    ellipse(x, y, radius*2, radius*2);
    textSize(24);
    textAlign(LEFT, CENTER);
    text("Score: " + score, 10, 30);
    text("Countdown: " + timer, 120, 30);

    if (frameCount % interval == 0 && timer > 0) { // if the frameCount is divisible by 60, then a second has passed. it will stop at 0
      timer --;
      newCircle();
    }
    if (timer === 0) { // set game over to true when timer runs out
      
      gameOver = true;
    }
  } else { // game over is true
    textSize(100);
    textAlign(CENTER, CENTER);
    text("GAME OVER", width/2, height/2);
    
  }
}
</code>
</pre>
</details>


```javascript

```
- 



<details>
<summary>Want to see the final code?</summary>
<br>
This is how you dropdown.
<br><br>
<pre>
<code>
function mousePressed() {
  
  if(!gameOver) {
      let d = dist(mouseX, mouseY, x, y);
       if (d < radius) {
          newCircle();
          score++;
        }
    } else {
      let d = dist(mouseX, mouseY, windowWidth/2,  windowHeight/2);
      if (d < radius*2) {
        gameOver = false;
        timer = 10;
        score = 0;
      }

    }
 
}
</code>
</pre>
</details>

### Task 6 - Stretch Goals

What kind of parameters from the objects in your sketch can you map to sound?

What about:

- Speed to volume? [HINT](https://p5js.org/reference/#/p5.SoundFile/setVolume)
- X Position to pan? [HINT](https://p5js.org/examples/sound-pan-sound.html)
- Y Position to filter frequency? [HINT](https://p5js.org/reference/#/p5.Filter)

etc etc etc

