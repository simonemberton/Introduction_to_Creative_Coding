# Session 03

## Changing behaviours based on input


### Task 1 - Repetition

Here is the code for the while loop we saw in the lecture

```javascript
function setup() {
  createCanvas(500, 500);
  background(0);
  fill(0, 0, 255);
}

function draw() {
let x = 25;
while (x < width){
	ellipse(x,25,50,50);
	x = x+50;
	}
}
```

Your task is to convert it into a for loop.  Do you remember the structure?

```javascript
for (init; test; update) {
    statements
}
```


### Task 2 - Repetition, Repetition

Now try and write a nested for loop so that ellipses are drawn across the whole of the canvas.

* Add random values to ```fill()``` to change the colour of the ellipse.  

* Move the location of ```fill()``` and see how it changes what's drawn to the canvas.


### Task 3 - Repetitive functions

Write a function that takes the x, y locations of your nested for loop as input.  Here is an example of my ```herringBone()``` function.

```javascript
function herringBone(x_, y_, unit) {
	stroke(0);
	strokeWeight(1);

	line(x_, y_, x_-unit, y_+unit);
	line(x_, y_, x_+unit, y_+unit);
	line(x_, y_-unit, x_-unit, y_);
	line(x_, y_-unit, x_+unit, y_);
	line(x_, y_-unit, x_, y_+unit);
	line(x_+unit, y_-unit, x_+unit, y_+unit);
} 
```

Don't forget to call the function inside the for loop!

* Try making at least three more functions each with a different pattern.  

* Try creating a new variable that increases/decreases on each frame which can also used as input to your function (e.g. to change the scale).

### Task 4 - Flip through functions

Finally, wrap it all up by using a switch/case statement which allows you to select which of the patterns you draw to the canvas.

```javascript
switch(key){
    case "1": 	
	function1(x,y,unit);
    break;
    case "2": 
    	function2(x,y,unit);
    break;
    case "3": 
    	function3(x,y,unit);
    break;
    case "4": 
    	function4(x,y,unit);
    break;
	    }
```
