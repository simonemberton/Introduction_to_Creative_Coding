# Week 16

### Code challenge 5: Create the minute hand of a clock / Rotation and angles

<p align="center">
<img src="./assets/analogue-clock.gif" alt="Drag" width="50%"/>
</p>

Your ```sketch.js``` should look as follows:  

```javascript
function setup() {
  createCanvas(600, 600);
  angleMode(DEGREES);
}

function draw() {
  background(0);
  translate(300, 300);      
  //setting the seconds 
  let s = second();      
  strokeWeight(4);
  stroke(color(255, 255, 255));
  noFill();
  // set the angle every second / s
  let angle = map(s, 0, 60, 0, 360);
  rotate(angle);
  line(0, 0, 200, 0);
}
```