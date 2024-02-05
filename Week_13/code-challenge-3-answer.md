# Week 13

## Code Challenge 3 - Answer

- Your final sketch.js code should now look like this 


```javascript
let video;
let detector;
let detections = [];
let start = false;
let rectWidth = 150;
let rectHeight = 60;
let state = "start";

function setup() {
  createCanvas(640, 480);
  background(220);
  video = createCapture(VIDEO, videoReady);
  video.size(640, 480);
  video.hide();
}



function draw() {
  
  switch (state) {
    case "video":
      videoUI();
      break;
    
    case "start":
      textUI("Start Here", 200,200,200, "Start");
      break;
    
    case "caught":
      textUI("Gotcha!", 255,0,0, "Start Again");
      break;

    case "success":
      textUI("Success!", 0,255,0, "Start Again");
      break;
    
  }
  
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

// mouse pressed on button
function mousePressed() {
  let rectX = (width - rectWidth) / 2;
  let rectY = (height - rectHeight) / 2;
  if (mouseX > rectX && mouseX < rectX+rectWidth && mouseY > rectY && mouseY < rectY+rectHeight) {
    console.log("hit");
    state = "video";
  } 
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

  // loop through the array of detections
  for (let i = 0; i < detections.length; i++) {
    // label is 0.9 person
    if(detections[i]["label"] == "person" && detections[i]["confidence"] > 0.9) {
      // you were caught - change the 'state' variable to caught
      state = "caught";
    }
    else if(detections[i]["label"] == "cell phone" && detections[i]["confidence"] > 0.8) {
      // you won - change the 'state' variable to success
      state = "success";
    }
    
  }
  detector.detect(video, gotDetections);
}


```
