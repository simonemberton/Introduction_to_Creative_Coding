# Week 09

## Tranformations, intersections and more particle system fun

### Task 1 - Transformations

Create a new sketch and run the following code:

```javascript

let angle = 0;

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

In the ```setup()``` function let's change the measure of angles from radians to degrees with the following ```angleMode(DEGREES);```.  Now our rectangle is rotating a bit more slowly.

Try writing some code to draw another spinning rectangle in the bottom righthand corner.

<details>
<summary>See here for a hint</summary>
  
  <p>https://p5js.org/reference/#/p5/push</p>
  
</details>

The end result of this task should look something like this:

<p align="center">
  <img width="500" height="499" src="./images/Task1.gif">
</p>

### Task 2 - Intersections

Create a new sketch and run the following code:

```javascript
let b1;
let b2;

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

You should be able to see two blue circles bouncing around the canvas.  Your task is to write some code inside the draw function which detects whether or not the two circles are intersecting and if they are change their colour (i.e. call the ```changeColor()``` method of the Bubble class).

<details>
<summary>See here for a hint</summary>
  
  <p>https://p5js.org/reference/#/p5/dist</p>
  
</details>

### Task 3 - Intersects

Once you've got that working, we want to move this code outside of the ```draw()``` function and put it into our ```Bubble``` class. Try writing a new method for the ```Bubble``` class called ```intersects()``` which checks for the intersecting circles and then call this method inside the ```draw()``` function.

If you're struggling with this task see the hint below to see what you need to add to the ```draw()``` function.

The result of this task should look something like this:

<details>
<summary>See here for a hint</summary>
  
  <p>
  ```javascript
  if (b1.intersects(b2)){
  	b1.changeColor();
	b2.changeColor();
  }
  ```
  </p>
  
</details>

<p align="center">
  <img width="498" height="498" src="./images/Task2_3.gif">
</p>

### Task 4 - Array of bubbles

At the moment we've only got two circles bouncing around.  Imagine we want to create a few more bubble objects but don't want to have to write a lot more lines of code.  Yes that's right we can use an array.  Please check back to the code from Task 5 of [Week_07](https://github.com/davemeckin/Intro_to_Creative_Programming/blob/master/Week_07/Week_07.md) if you can't remember how to do this. Let's create an empty array at the top of our sketch, include a for loop inside the ```setup()``` function which fills our array with new Bubble objects and in the ```draw()``` function write another for loop which calls the ```move()``` and ```display()``` methods.  

You'll notice now that the ```changeColor()``` isn't working as we intended.  We need to add another loop so that we can check through both arrays to see if there are any intersections.  Don't forget we don't want to check if a circle is intersecting with itself! 

If you got that working you should now be able to create as many bubble objects as you like.  In my code there are five and it looks like this:

<p align="center">
  <img width="499" height="497" src="./images/Task4.gif">
</p>

### Task 5 - Spin

Now let's put together our transformations and intersections.  Instead of changing the colour of the circles when they intersect we want to rotate them.  Obviously you can't see a whether or not a circle is rotating so first off draw a line through the middle of the circle. You'll need to use ```push()```, ```translate()``` and ```pop()``` to do this.

Next create a new variable in Bubble's constructor ```this.angle = 1;``` and a new method of our Bubble class called ```spin()``` as follows:

```javascript
spin(val) {
		this.angle = this.angle+val;
	}
```

Now try and add some code to the ```display()``` method which will do the rotating and finally call the ```spin()``` method when any circles intersect with each other.

If you've completed this task your canvas should look something like this:

<p align="center">
  <img width="497" height="497" src="./images/Task5.gif">
</p>

Well done for getting to the end of the worksheet!
