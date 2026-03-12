# Week 18

Code Challenge 1:  

Output if is the left or right hand using hands[0].handedness.   
Change the colour of the circles for the left and right hand.  

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

  // Check that handpose can see one or more hands
  if (hands.length > 0) {
    // get left or right hand
    console.log(hands[0].handedness);
    // loop through the result getting all the keypoints
    for (let i = 0; i < hands[0].keypoints.length; i++) {
      let keypoint = hands[0].keypoints[i];
      fill(255, 255, 0);
      // change circle colour based on hand left or right
      if(hands[0].handedness == "Left") {
        fill(0, 255, 0);
      }
      noStroke();
      // draw a circle using the x, y keypoints
      circle(keypoint.x, keypoint.y, 16);
    }
  }

}
```