# Session 07

## Tranformations, intersections and more particle system fun

### Task 1 - Transformations

Create a new sketch and run the following code

```javascript

var angle = 0;

function setup(){
	createCanvas(500,500);
}

function draw() {
	background(0);

	translate(100,100);
	rotate(angle);
	fill(255,0,0);
	rectMode(CENTER);
	rect(0,0,100,50);
	
	angle = angle + 1;
}
```

You should be able to see a red rectangle in the top lefthand corner spinning really fast!  The reason it's spinning so fast is that we're using the default input to the ```angle``` function which is radians and there are only 2`*`pi (around 6.2813) radians in a circle.

In the ```setup()``` function let's change the measure are angles to degrees with the following ```angleMode(DEGREES);```.  Now our rectangle is rotating a bit more slowly.

Try writing some code to draw another spinning rectangle in the bottom righthand corner. 

[HINT](https://p5js.org/reference/#/p5/push)

### Task 2 - Intersections

Create a new sketch and run the following code

```javascript
var b1;
var b2;

function setup(){
	createCanvas(500,500);
	b1 = new Bubble(random(50,width-50),random(50,height-50));
	b2 = new Bubble(random(50,width-50),random(50,height-50));
}

function draw() {
	background(0);
	b1.move();
	b2.move();
	b1.display();
	b2.display();
}


class Bubble {
	
	constructor(startX, startY){
		
		this.x = startX;
		this.y = startY;
		this.r = 50;
		this.col = color(0, 0, random(125,255));

		this.xSpeed = random(0.3,5);
		this.ySpeed = random(0.3,5);
		this.xDir = 1;
		this.yDir = 1;

	}

	changeColor() {
		this.col = color(0, 0, random(125,255));
	}

	move() {
		
		this.x = this.x + (this.xSpeed * this.xDir);
		this.y = this.y + (this.ySpeed * this.yDir);

		if(this.x > (width-(this.r)) || this.x < (this.r)){
			this.xDir = this.xDir * -1;
		}

		if(this.y > (height-(this.r)) || this.y < (this.r)){
			this.yDir = this.yDir * -1;
		}

	}

	display() {
		noStroke();
		fill(this.col);
		ellipse(this.x, this.y,this.r*2,this.r*2);
	}

}
```

You should be able to see two blue circles bouncing around the canvas.  Your task is to write some code inside the draw function which detects whether or not the two circles are intersecting and if they are change their colour (i.e. calls the ```changeColor()``` method of the Bubble class). 

### Task 3 - Intersects

Once you've got that working, we want to move this code outside of the draw function and put it into our Bubble class. Try writing a new method for the Bubble class called ```intersects()``` which checks for the intersecting circles and call this method inside the draw function.

### Task 4 - Array of bubbles

At the moment we've only got two circles bouncing around.  Imagine we want to create a few more bubble objects but don't want to have to write a lot more lines of code.  Let's create an empty array at the top of our sketch, include a for loop inside the ```setup()``` function which fills our array with new Bubble objects and in the ```draw()``` function write another for loop which calls the ```move()``` and ```display()``` methods.  

You'll notice now that the ```changeColor()``` isn't working as we intended.  We need to add another loop so that we can check through both arrays to see if there are any intersections.  Don't forget we don't want to check if a circle is intersecting with itself!

### Task 5 - Spin

Now let's put together our transformations and intersections.  Instead of changing the colour of the circles when they intersect we want to rotate them.  Obviously you can't see a whether or not a circle is rotating so first off draw a line through the middle of the circle.

Next create a new variable in Bubbble's constructor ```this.angle = 1;``` and a new method of our Bubble class called ```spin()``` as follows

```javascript
spin(val) {
		this.angle = this.angle+val;
	}
```

Now try and add some code to the ```display()``` method which will do the rotating and finally call the ```spin()``` method when any circles intersect with each other.













