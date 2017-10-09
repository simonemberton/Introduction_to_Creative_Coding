# Session 02

## Basic Interactivity


### Task 1 - Drawing with the mouse

Okay let's start with our setup and draw functions.  In ```setup()``` as well as our ```createCanvas()``` function we've got [```fill()```](https://p5js.org/reference/#/p5/fill) and [```noStroke()```](https://p5js.org/reference/#/p5/noStroke).  Check out the links to the reference pages if you're not sure what these do.

c

Can you fill in the values inside the ellipse so that it is drawn at the mouse's location on the screen?

[HINT](https://p5js.org/reference/#/p5/mouseX)


### Task 2 - Draw a continuous line

Now we're going to try and draw a continuous line!

```javascript
function setup(){
  createCanvas(500,500);
  strokeWeight(4);
  stroke(0, 102);
}


function draw() {
  line();
}
```

Inside draw function we're going to use the ```line()``` function.

Notice how we now need to specify [```strokeWeight()```](https://p5js.org/reference/#/p5/strokeWeight) and [```stroke()```](https://p5js.org/reference/#/p5/stroke) as we're drawing a line.  Again have a look at their reference pages if you're not sure what they do.

If you look at line()'s reference [page](https://p5js.org/reference/#/p5/line) you'll notice it takes four location values.  Can you remember a way to input the previous location of the mouse?

[HINT](https://p5js.org/reference/#/p5/pmouseX)


### Task 3 - Draw fluidly

In the lecture we talked about 'easing'.  This technique can be used to make our drawn object lag behind the location of the mouse and makes for a more fluid drawing style.

Try this code:

```javascript

var x = 0;
var easing = 0.01;

function setup(){
  createCanvas(500,500);
}


function draw() {
	var targetX = mouseX;
    x += (targetX - x) * easing;
  	ellipse(x, 40, 12, 12);
    print(targetX + " : " + x);
}
```

* See how the numbers that are output to the console change as you move and stop moving.
  
* Try changing the value of the ```easing``` variable and see how it changes the movement.

* Add some code so that the easing also happens on the Y axis.


### Task 4 - Conditionals

For this task you need to make use of ```mouseIsPressed``` and an ```if``` statement.  If the mouse is pressed make something happen e.g. the screen changes colour or an object is drawn.

[HINT](https://p5js.org/reference/#/p5/mouseIsPressed)
