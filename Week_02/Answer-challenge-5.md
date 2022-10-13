# Week 02

## Code Challenge 5 - Answer

Hopefully your sketch looks like a bit like this:  

<p align="center">
<img src="./images/code-challenge-5.png" alt="ellipse" width="50%"/>
</p>


And your whole sketch code should look something like this:  

```javascript
let rectWidth = 100;
let rectHeight = 125;


function setup() {
  createCanvas(800,600);
	background(200);
	noLoop();
}

function draw() {
  noFill();
	drawShape(rectWidth,rectHeight);
}


function drawShape(rectangleWidth,rectangleHeight) {
	let xPos = random(0, width);
	let yPos = random(0, height);

	rect(xPos,yPos,rectangleWidth,rectangleHeight);
}
```
