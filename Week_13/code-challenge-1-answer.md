# Week 13

## Code Challenge 1 - Answer

- Your final sketch.js code should look like this with the confidence value rounded to 2 decimal places and a red bounding box (or another colour you choose)

```javascript
let video;
let detector;
let detections = [];

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO, videoReady);
  //video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
}



function draw() {
  image(video, 0, 0);

  for (let i = 0; i < detections.length; i += 1) {
    const object = detections[i];
    stroke(255, 0, 0);
    strokeWeight(4);
    noFill();
    rect(object.x, object.y, object.width, object.height);
    noStroke();
    fill(255);
    textSize(24);
    text(object.label +" "+ round(object.confidence, 2), object.x + 10, object.y + 24);
  }
}

function videoReady() {
  // Models available are 'cocossd', 'yolo'
  detector = ml5.objectDetector('cocossd', modelReady);
}

function modelReady() {
  detector.detect(video, gotDetections);
}

function gotDetections(error, results) {
  if (error) {
    console.error(error);
  }
  detections = results;
  console.log(detections);
  detector.detect(video, gotDetections);
}
```
<p align="center">
<img src="./images/mlgame-detections.jpg" alt="me" width="100%"/>
</p>