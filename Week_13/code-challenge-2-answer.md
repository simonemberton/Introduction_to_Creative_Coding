# Week 13

## Code Challenge 2 - Answer

- Your final sketch.js code should now look like this 
- you should be able to uncomment the lines in ```draw()``` one by one to see each screen running.  

```javascript
let video;
let detector;
let detections = [];
let rectWidth = 150;
let rectHeight = 60;

function setup() {
  createCanvas(640, 480);
  background(220);
  video = createCapture(VIDEO, videoReady);
  video.size(640, 480);
  video.hide();
}



function draw() {
  //un-comment each line below one by one to see each screen running
  
  //videoUI();

  //textUI("Start Here", 200,200,200, "Start");

  //textUI("Success!", 0,255,0, "Start Again");
  
  //textUI("Gotcha!", 255,0,0, "Start Again");
  
}

// show text screen
function textUI(msg, r,g,b, btnText) {
  background(r,g,b);
  let rectX = (width - rectWidth) / 2;
  let rectY = (height - rectHeight) / 2;

  // Draw the centered button rectangle
  strokeWeight(1);
  stroke(255);
  fill(r,g,b);
  rect(rectX, rectY, rectWidth, rectHeight);

  // Set up text properties
  textAlign(CENTER, CENTER);
  textSize(20);
  fill(255); // White text

  // Calculate the position for the centered text
  let textX = width / 2;
  let textY = height / 2;

  // Draw centered button text
  text(btnText, textX, textY);
  // Draw alert text
  textSize(60);
  text(msg, textX, textY-100);
}


// show video detections
function videoUI() {
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
    text(object.label, object.x + 10, object.y + 24);
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
