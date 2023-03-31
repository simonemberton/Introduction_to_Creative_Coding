# Week 16

## Answer - Task 2 - Adding a mouse press - First code challenge



- Your sketch.js code should like this to pause and play our song based on clicking with a mouse button press.

```javascript
let song;

function preload() {
  song = loadSound("piano.mp3");  
}

function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(480, 270);
  background(0);
  fill(255);
  textAlign(CENTER);
  text("click canvas to play", width/2, height/2);
  noLoop();
  console.log("hi");
}

function mousePressed() {
  userStartAudio();
  if (song.isPlaying()) {
    song.stop();
  } else {
    song.play();
  }
}

//function draw() {
//}
```
