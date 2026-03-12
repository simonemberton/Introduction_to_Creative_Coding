# Week 18

Code Challenge 2:  

Use the dist() function to calculate the distance between the index and thumb.
Console.log() the result (distance)
HINT: the index and thumb both have x, y properties index.x etc

Sketch.js should look like this:

```javascript
let video;
let handPose;
let hands = [];

function preload() {
  // Initialize HandPose model with flipped video input
  handPose = ml5.handPose({ flipped: true });
}

function mousePressed() {
  console.log(hands);
}

function gotHands(results) {
  hands = results;
}

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO, { flipped: true });
  video.hide();

  // Start detecting hands
  handPose.detectStart(video, gotHands);
}

function draw() {
  image(video, 0, 0);

  // Ensure at least one hand is detected
  if (hands.length > 0) {
    console.log(hands[0].handedness);
    // get the index finger tip and thumb tip (look at the console results for the names)
    let index = hands[0].index_finger_tip;
    let thumb = hands[0].thumb_tip;
    // set the fill color
    fill(255,255,0); 
    noStroke();
  
    // use the dist function to get the proximity of both
    let d = dist(index.x, index.y, thumb.x, thumb.y);
    console.log(d);

    // draw the circles
    circle(index.x, index.y, 20);
    circle(thumb.x, thumb.y, 20);
  
  }
}
```