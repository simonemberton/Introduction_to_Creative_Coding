# Week 16

## Answer - Task 2 - Adding a mouse press - First code challenge



- Your sketch.js code should like this to pause and play our song based on clicking within the circle.

```javascript
let song;
let circleX = 130; // x-coordinate of the center of the circle.
let circleY = 30; // y-coordinate of the center of the circle.
let circleD = 120;// Number: diameter of the circle.

function preload() {
  song = loadSound("piano.mp3");  
}

function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(480, 270);
  background(0);
  fill(255);
  circle(circleX, circleY, circleD);
  noLoop();
  console.log("hi");
}

function mousePressed() {
  let distance = dist(circleX, circleY, mouseX, mouseY);
  console.log(distance);
  userStartAudio();
  if (song.isPlaying() && distance < circleD/2) {
    song.stop();
  } else if (distance < circleD/2) {
    song.play();
  }
}
```
