# Week 16

## Answer - Task 1 - Adding a mouse press - First code challenge



- Your sketch.js code should like this to catch the space bar being pressed

```javascript
let bird;

function setup() {
  createCanvas(640, 480);
  bird = new Bird();
}

function draw(){
  background(0);
  bird.show();
  bird.update();
}


function keyPressed() {
  if (key == ' ') {
    console.log("pressed space");
    bird.up();
  }
}
```
