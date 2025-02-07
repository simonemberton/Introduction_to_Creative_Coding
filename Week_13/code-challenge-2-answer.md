# Week 13

## Code Challenge 2 - Answer

- Your final sketch.js code should now look like this, with a start screen and Gotcha and You Win screens all working


```javascript
let video;
let detector;
let detections = [];
let state = "start";

function setup() {
  createCanvas(640, 480);
  background(220);
  video = createCapture(VIDEO, videoReady);
  //video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
}


function draw() { 
  switch (state) {
    case "start":
      start();
      break;

    case "video":
      videoUI();
      break;
    
    case "caught":
      gotcha();
      break;

    case "success":
      youWin();
      break; 
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
    text(object.label +" "+ round(object.confidence, 2), object.x + 10, object.y + 24);
  }
}

// Start screen
function start(){
  background(0,0,0);
  // Set up text properties
  textAlign(CENTER, CENTER);
  textSize(60);
  fill(255); // White text
  text("Press space to start", width/2, height/2);
}

// Gotcha screen
function gotcha(){
  background(255,0,0);
  // Set up text properties
  textAlign(CENTER, CENTER);
  textSize(60);
  fill(255); // White text
  text("Gotcha!", width/2, height/2);
}

// you win screen
function youWin(){
  background(0,255,0);
  // Set up text properties
  textAlign(CENTER, CENTER);
  textSize(60);
  fill(255); // White text
  text("You Win!", width/2, height/2);
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
 
  // Loop through the detections array [] and test the results for "person" and "cell phone"
  for (let i = 0; i < detections.length; i++) {
    //console.log(detections[i]["label"]);
    // label is 0.9 person
    if(detections[i]["label"] == "person" && detections[i]["confidence"] > 0.8 && state == 'video') {
      // you were caught
      console.log("caught");
      state = "caught"; // change the 'state' variable to caught
    }
    else if(detections[i]["label"] == "cell phone" && detections[i]["confidence"] > 0.3 && state == 'video') {
      // you won 
      console.log("success");
      state = "success"; //change the 'state' variable to success
    }
  }
  detector.detect(video, gotDetections);
}

// if space bar pressed change state to video or back to start
function keyPressed() {
  if (key === ' ' && state == 'start') {
    state = "video";
  }
  else {
    state = "start";
  }
}


```
