# Week 02

## Anatomy of a Sketch


## Task 0 - Download the p5.zip folder on the link p5complete.zip from p5js.org

- Hopefully you already did this last week! 

- Then copy the folder called "empty-example" **in the same enclosing folder**, rename it to something like "tutorial-2".

- Go into your newly copied folder and open both index.html and sketch.js in a text editor. VS Code is recommended.

- Remember, every time you change the code in your sketch.js file, you will need to refresh your browser to see the changes. 


## Task 1 - Ready, setup(); and draw();

#### As discussed in our lecture, what are the two most basic building block functions to get our p5 sketch going?

That's right, it's our old friends:

```javascript
function setup(){

}


function draw() {

}
```
### **Please do not copy and paste code - you will only learn by typing it out correctly**

#### What do we need to draw on? 

Paper? A wall? Or perhaps a canvas:

```javascript
createCanvas(800,600);
```

Where do you need to put this createCanvas function?

What do the values 800 and 600 relate to?

Remember to add the semi-colon at the end to signify the end of the statement!

FYI if you don't define this explicitly, a default canvas will be made with size 100 x 100.

#### Setting background

The other thing we need to do is set the background colour. Try putting this in the ``` setup() ``` function

```javascript
background(0);
```

 or 

 ```javascript
background(255);
```

What's the difference? Why do you think this is?

#### Draw a basic shape

```javascript
function setup(){

}


function draw() {

}
```

Try adding the following code to your ```draw()``` function:

```javascript
text("Hello World!", 0,50);
```
It should look like this:

```javascript
function draw() {
	text("Hello World!", 0,50);
}
```

This is your first computer program, nice one!

Now also add a circle to your ```draw()``` function:

```javascript
ellipse(50,50,40,40);
```
## &#x1F536; Code Challenge 1:

```diff
! Working in pairs...
! One person reads and explains, the other person types in the code.

! Using the p5 reference (link below)
! Find the entry for text()
! Work out what the parameters being passed to the text() function relate to
! You may also have to refer to the 'Coordinates' example in the p5 examples (link below)

! Make the text display on top of the ellipse (so you can see it)
! Now position the text in the middle of the sketch
! Change what the text says

! Using the p5 reference 
! Find out what the numbers being passed to the ellipse function mean.
! Make an ellipse that touches all the sides of your canvas

! Using the p5 reference
! Work out how to colour the text and increase the text size
```
p5 reference [https://p5js.org/reference/](https://p5js.org/reference/)   
p5 examples [https://p5js.org/examples/](https://p5js.org/examples/)


### Task 2 - All the 2D Shapes, well, quite a few anyway...


Have a look on [here](https://p5js.org/reference/) at the 2D primitives section to find out more info about each of the shape functions and their parameters.

Add a line to your sketch at the bottom of the ```draw()``` function.  

```javascript
rect(0,50,400,70);
```
## &#x1F536; Code Challenge 2:

```diff
! Working in pairs... Reverse roles
! One person reads and explains, the other person types in the code.

! Using the p5 reference 
! Find 2D primitives section
! Your task now is to try adding the following shape functions to your code one by one.
! triangle()
! quad()
! line()
! arc()
! Read the reference for each shape to work out how to include it

! If you manage that quickly using the p5 reference find some more shapes to add.
! Try changing the colour of each shape (refer to how you changed the ellipse colour)
```

Also experiment with the ```noFill()``` and ```fill(0)``` functions to see what happens. Try placing them on different lines of the sketch within the ```draw()``` function. What do you think fill is referring to? What happens when you put the function calls on different lines?


### Task 3 - Variables

Up to now, we've just been typing the values into the function calls. This can get annoying when we want to change stuff on the fly, based on user interaction, or a whole host of other reasons.

So, we're going to make use of a programming construct called a **variable**. A variable is essentially a space in the computer's memory that can be used to store data. Data could be lots of different things (we'll talk about this later on in the module) but here we are going to store numbers. 

Make sure you have a

```javascript
rect(25,50,25,25);
```

statement in your ```draw()``` function.

We declare a variable by using the ```let``` keyword. And when we declare it, we have to give it a **unique** name. 

At the very top of your sketch.js file, on the first line before ```setup()```, write the following:

```javascript
let rectWidth = 25;
let rectHeight = 25;
```

Now, update your ```rect()``` function as follows:

```javascript
rect(25,50,rectWidth,rectHeight);
```
See any change? No, because we've actually set them the same as they were before! 

But now try changing ```rectWidth``` to something big like 500. Now see a change?

## &#x1F536; Code Challenge 3:

```diff
! Working in pairs... Reverse roles
! One person reads and explains, the other person types in the code.

! Add 3 more rect() shapes inside the draw() function. 
! Use the rectWidth,rectHeight variables with these new rect() shapes.
! Make sure they are at different coordinates (the first two values) from each other so you can see them.
! Now try changing the rectWidth and rectHeight variable values. What happens? 
```

Can you see that all the functions are sharing the value stored within the variables?


### Task 4 - Operators (+ some maths)

#### **Operators** in programming (and as such JavaScript/p5.js) are used to do maths, assign values to variables, compare variables and a whole lot more. 

#### In this task, we're going to use some simple arithmetic operators:


- ``` + ``` for addition.
- ``` - ``` for subtraction.
- ``` * ``` for multiplication.
- ``` / ``` for division.

Now we have our ```rectWidth``` and ```rectHeight``` variables. We can start to manipulate them in different parts of the program. 

- Try adding ```10``` to the height of the first rectangle. 

- Try subtracting ```10``` from the width of the second rectangle. 

- Try multiplying the height of the third rectangle by ```2```. 

- Try dividing the width of the fourth rectangle by ```2```. 

Now try messing around with other values, what happens when you multiply something by a value less than 1 for instance?

### Task 5 - Make it Functional

OK, the last piece of our abstract thinking puzzle for the day. Remember that functions are reusable blocks of code? Well this is going to involve us "wrapping" some functionality into a function, so we can call it over and over. We'll also be looking at a new function called ```random()```.

- First of all, in the ```setup()``` function, add the line:

```javascript
noLoop();
```
So your ```setup()``` function should now look like this:

```javascript
function setup(){
	createCanvas(800,600);
	background(255);
	noLoop();
}
```
This just means our ```draw()``` function is only called once now, like ```setup()``` is.

Now, beneath the ```draw()``` function, write a new function called "drawShape". Add two parameters that will be passed to this function called width and height:

**remember the syntax of how to write this stuff, it has to be exactly correct otherwise the script interpreter won't know what to do with it**

```javascript
function drawShape(rectangleWidth,rectangleHeight) {

}
```

OK, so here is where we're going to stick some randomness into our function. Add the following lines into the top of the function:

```javascript
	let xPos = random(0, width);
	let yPos = random(0, height);
```

so it looks like this:

```javascript
function drawShape(rectangleWidth,rectangleHeight) {
	let xPos = random(0, width);
	let yPos = random(0, height);

}

```

What we're doing here is declaring a new random number in the range between 0 and the width or height. 

OK, now just add one more line into your ```drawShape()``` function:

```javascript
	rect(xPos,yPos,rectangleWidth,rectangleHeight);
```

So, now your function should look like this:

```javascript
function drawShape(rectangleWidth,rectangleHeight) {
	let xPos = random(0, width);
	let yPos = random(0, height);

	rect(xPos,yPos,rectangleWidth,rectangleHeight);
}
```

Now you can call ```drawShape()``` from within ```draw()``` using the following code:

```javascript
function draw() {
	noFill();
	drawShape(rectWidth,rectHeight);

	}
```

Try refreshing the page over and over. What is happening? Are you seeing a new rectangle in a different position each time?

Try multiplying rectWidth and rectHeight by some small numbers like 2 or 3. Remember to keep refreshing to see the shape drawn in a different random spot.

### Task 6 - Basic Procedural Drawing

OK, it's important you add the following bits of code into the ```draw()``` function line by line and refresh the page each time to see what's going on... (and please don't copy once again)

```javascript
	rectMode(CENTER);
	rect(100,100,20,100);
```
Then

```javascript
	ellipse(100,70,60,60);
```
Then

```javascript
	ellipse(81,70,16,16); 
```
Then

```javascript
	ellipse(119,70,16,16); 
```
Then

```javascript
	line(90,120,80,105);
```
Then

```javascript
	line(110,120,120,105);
```

Then

```javascript
	line(90,150,80,160);
```
Then

```javascript
	line(110,150,120,160);
```


### Task 7 - Repetition

Try repeating the draw shape function call loads of times by copying and pasting. Make sure you have different multiplication values each time.

### Task 8 - Play

Try experimenting with different shapes (ellipse/rect/triangle), fills and multiplication values to see what kind of patterns and pictures you can create with these simple geometric shapes!

For instance, what happens when you do this:

```javascript
fill(random(0,255),random(0,255),random(0,255));
```

Also, what happens when you take out ```noLoop()```?








