# Week 06

## Object Orientation in JavaScript



<p align="center">
  <img width="500" height="400" src="./images/classes.png">
</p>


### Task 1 - Starting Point Sketch


Create a blank sketch with a canvas 1200px wide and 800px high.

Set the background to white.

*Below* the ```draw``` function, create two new functions, one called ```move()``` and one called ```display()```.

What we're going to build first, is one rectangle in the centre of the screen. Seems simple enough but we're also going to add some bits that we'll use later.

First of all, we need to define a bunch of variables. We need them for the x and y positions; the size of our shape; and half the size of our shape. So declare the following at the very top of the sketch:

```javascript
let x,y,size;
```

Then directly below, we're going to declare some variables for later on once we get our rectangle moving:

```javascript 
let xSpeed,ySpeed,xDir,yDir;
```
These will relate to the speed of the object as it moves and whether it is travelling in a positive direction (to the right on the x axis and down on the y) or in a negative direction (to the left on the x axis and up on the y).

In ```setup()```, we're going to initialise our variables. First, we'll do the position and size:

```javascript 
x = width/2; //middle
y = height/2; //centre
size = 10;
```

Now, we're going to initialise our xSpeed and ySpeed to random numbers between 0.3 and 5. And our 

```javascript 
xSpeed = random(0.3,5);
ySpeed = random(0.3,5);
xDir = 1;
yDir = 1;
```
OK it would be good to actually see something now. In the ```display()``` function you created earlier, let's make it draw our rectangle:

```javascript
stroke(10);
rectMode(CENTER);
fill(0);
rect(x, y,size,size);
```
OK, now call ```display()``` from ```draw()``` and you should see you shape in the middle of the screen when you refresh the page.


### Task 2 - Moving

OK, here goes the animation part. There will be logic and there will be maths, please shout someone if you want clarification.

So first of all we want to move our shape. In order to do this, we need to change the x and y positions of the shape every time a new frame happens. So we're going to call the ```move()``` function from the ```draw()``` function. Make sure you add this *before* the call to ```display()```.

Now, in ```move()```, we're going to add some simple maths to add to our shape's x and y positions every time it is called (don't copy):

```javascript
x = x + (xSpeed * xDir); //add xSpeed multiplied by xDir (positive 1 or negative 1)
y = y + (ySpeed * yDir); //add ySpeed multiplied by yDir (positive 1 or negative 1)
```
Save the sketch and refresh the page. Your shape will likely move down and to the right and then off the canvas never to been seen again!

OK so you see the issue we have here? We need to *constrain* the movement of our shape. And we do this by use if statements. We'll also introduce the fact that if statements can contain more than one condition. For instance we could say "if this AND that happen, do this". Or in our case, we're going to say "if this OR that happen, do this". The symbol for OR in computer programming is ```||```.

So let's write two if statements first:

```javascript
if(x > (width-size) || x < size){ // if x is greater than the width (minus size) OR if x is less than size

}

if(y > (height-size) || y < size){ // if y is greater than the height (minus size) OR if y is less than size

}
```

What we're doing here is setting up conditional statements that are constantly testing whether our shape's position is hitting the boundaries of our canvas. Now we need to put in some code that changes the direction of travel inside our if statements. If we want to change direction, we need to change our xDir and yDir variables from a negative to a positive, or vice versa. That way, if our shape is travelling right (positive) and reaches the right hand edge of the screen, we can change xDir to -1 so that means xSpeed * xDir will be a negative value and our shape will start travelling left. The same goes for the y value...

```javascript
if(x > (width-size) || x < size){
            xDir = xDir * -1; // flip between positive 1 and negative 1
            
}

if(y > (height-size) || y < size){
            yDir = yDir * -1; // flip between positive 1 and negative 1
}
```
So your if statements in your ```move()``` function should now look like the above. And your whole ```move()``` function should look like below:

```javascript
function move() {
        
        x = x + (xSpeed * xDir);
        y = y + (ySpeed * yDir);

        if(x > (width-size) || x < size){
            xDir = xDir * -1;
            
        }

        if(y > (height-size) || y < size){
            yDir = yDir * -1;
        }
}
```
Save and refresh, how's it looking?! Can you see a shape moving around the screen? Ask for assistance if not...


### Task 3 - VFX

So it's mildly visually uninteresting right now. Let's just add a little bit of a vapour trail to the movement to show direction of travel. In order to do this, we're going to put an overlay fade on the whole canvas.

There are loads of ways to do this, but our way is by drawing a slightly transparent rectangle over everything else.

At the very top of your ```draw()``` function, add the following code:

```javascript
noStroke(); // no outline
rectMode(CORNER); // draw from top left corner
fill( 255, 255, 255, 80); //white 80 on the alpha channel
rect(0, 0, width, height); // draw a rectangle over the whole canvas
```

Try changing the alpha value to something lower than 80. Now try something higher... See what's happening?


### Task 4 - Object Orientation-ifying...

So this is kind of cool and all, but what if we wanted 20 rectangles all moving at different speeds in different directions? Or 200? or 2000? And what if we want some to be rectangles, some to be circles? With different colours or something? I personally can't be bothered to write all that out 2000 times. And imagine trying to find any errors or updating your code if you did do that?!

This is where our Object Oriented approach comes into it's own. Instead of writing out the same code over and over in order to get individual variables of x,y speed etc etc, we can *encapsulate* all the stuff we need into one nice package: let's call it a MovingShape.

Now, in order for us to that, we need to shift our moving and displaying functions around a bit and put them into the thing discussed in the lecture called a ```class```. A class is like a blueprint or outline of the object that we are going to make.


First of all, let's define our MovingShape class. At the bottom of your sketch, add the following:

```javascript
class MovingShape {

}
```

Now, if you remember, every class needs a ```constructor``` method. This gets called every time we make a new version of MovingShape using the keyword ```new```. More on that later, but for now let's put an empty constructor in:

```javascript
class MovingShape {
    
    constructor(){

    }
}
```

Let's also define our ```move()``` and ```display()``` functions in our class definition:

```javascript
class MovingShape {
    
    constructor(){

    }

    move(){

    }

    display() {

    }
}
```
See what we mean about creating a blueprint? What's going to happen is that when we create a new *instance* of the class MovingShape, it will have its own move and display methods that only relate to it.

OK, here's the mildly mind mangly bit: we now have methods that only relate to the instance of the class, but we need to make sure that all our variables also only relate to that instance. As you know, at first we defined all our variables in global scope at the top of the sketch. But now we need to change that. And that is where the ```this``` keyword comes in. Using the ```this``` keyword allows us to create variables that only belong to the object instance. So now, ```this.x``` will relate only to a single instance of our MovingShape object. From your ```setup()``` function, paste in your initialised variables into your MovingShape constructor method. Then add ```this.``` to the beginning of all the variable names. Remember the DOT operator means that we're accessing a property belonging to ```this```. And ```this``` is the object instance:


```javascript
class MovingShape {
    
    constructor(){
        this.x = width/2;
        this.y = height/2;
        this.size = 10;
        this.xSpeed = random(0.3,5);
        this.ySpeed = random(0.3,5);
        this.xDir = 1;
        this.yDir = 1;
    }

    move(){

    }

    display() {

    }
}
```
OK now let's update our ```move()``` and ```display()``` methods so that the variables inside them relate to ```this``` instead of the global ones we defined earlier. We're just adding ```this.``` to the beginning of all the variable names. We're also going to divide size by two now so look out for this change:


```javascript
move() {
        
        this.x = this.x + (this.xSpeed * this.xDir);
        this.y = this.y + (this.ySpeed * this.yDir);

        if(this.x > (width-(this.size/2)) || this.x < (this.size/2)){
            this.xDir = this.xDir * -1;
            
        }

        if(this.y > (height-(this.size/2)) || this.y < (this.size/2)){
            this.yDir = this.yDir * -1;
        }

    }

    display() {
        stroke(10);
        rectMode(CENTER);
        fill(0);
        rect(this.x, this.y,this.size,this.size)
    }
```

So, your MovingShape class should look a little something like this now:

```javascript
class MovingShape {
    
    constructor(){
        this.x = width/2;
        this.y = height/2;
        this.size = 10;
        this.xSpeed = random(0.3,5);
        this.ySpeed = random(0.3,5);
        this.xDir = 1;
        this.yDir = 1;
    }

    move() {
        
        this.x = this.x + (this.xSpeed * this.xDir);
        this.y = this.y + (this.ySpeed * this.yDir);

        if(this.x > (width-(this.size/2)) || this.x < (this.size/2)){
            this.xDir = this.xDir * -1;
            
        }

        if(this.y > (height-(this.size/2)) || this.y < (this.size/2)){
            this.yDir = this.yDir * -1;
        }

    }

    display() {
        stroke(10);
        rectMode(CENTER);
        fill(0);
        rect(this.x, this.y,this.size,this.size)
    }

}
```

Alright, so final part of this section, let's make an object, or two!

In order to do that, we need to make a variable. Near the top of the sketch, in global scope, define a variable called shapey1:

```javascript
let shapey1;
```

In ```setup()```, let's assign an instance of our MovingShape object (the class that we have defined) by using the ```new``` keyword:

```javascript
shapey1 = new MovingShape();
```

OK now in ```draw()```, beneath our transparent rectangle code, let's move and display shapey1:

```javascript
    shapey1.move();
    shapey1.display();
```

So we're using the DOT operator to call the methods ```move()``` and ```display()``` that only relate to shapey1.

OK, now it's your turn, define shapey2 and assign a new object instance to it. Then add the correct lines for moving and displaying shapey2...

What happens if we want more shapes though?



<p align="center">
  <img width="500" height="400" src="./images/capture1.JPG">
</p>


### Task 5 - An Array of Objects

So we've *encapsulated* our code, and we've created a couple of instances that have their own related properties which allow them to move indepently of each other. Let's make 200 of them then! 

Remember how we were filling arrays last time? We can do exactly the same thing here. So while we had an initial bit of pain writing our class, we can now reuse that class over and over without having to write loads more code.

Let's define a global array. At the top of the sketch:

```javascript
let shapeArr = [];
```

Then in ```setup()```, let's add our objects:

```javascript
for(let i = 0; i < 200; i++) {
        shapeArr.push(new MovingShape());  // add a new MovingShape to our array each loop     
}
```
Now in ```draw()``` let's iterate through the array to move and display all of the objects inside the array on every frame:

```javascript
for(let i = 0; i < 200; i++) {
        shapeArr[i].move();
        shapeArr[i].display();
}
```

Whoa.


<p align="center">
  <img width="500" height="400" src="./images/capture.jpg">
</p>

### Task 6 - Passing Arguments to the Constructor

So we're cooking on gas now, but we can see that all our objects start from the same position. It would be cool if they all started from different positions. And also if they had different sizes.

Let's go ahead and update our constructor method so that we can pass variables into it when the object is created:

```javascript
class MovingShape {
    
    constructor(startX, startY, startSize){
        this.x = startX;
        this.y = startY;
        this.size = startSize;

        //etc etc
    }

    //etc etc
}
```
Now we need to update our ```setup()``` function so that the initialisation for loop passes random positions and sizes

```javascript
for(let i = 0; i < 200; i++) {
        shapeArr.push(new MovingShape(random(0,width),random(0,height),random(1,40)));  // add a new MovingShape to our array each loop at random pos and with random size   
}
```


<p align="center">
  <img width="500" height="400" src="./images/capture3.jpg">
</p>


### Task 7 - The Inheritors

Finally, to demonstrate the concept of *inheritence*, let's make a new class beneath our MovingShape class. We're going to use the ```extends``` keyword when defining a new class of MovingCircle. This time we're going to define the colour in the constructor:

```javascript
class MovingCircle extends MovingShape {
    
    constructor(startX, startY, startSize, colour){
     
    }

    move() {
     
    
    }

    display() {
    
    }

}
```
Now, we don't actually need to write out all the same code again. Because MovingCircle inherits properties and methods from MovingShape, all we need to do is use the ```super``` keyword to access the parent methods. We're also adding a variable called ```this.colour``` and overriding our ```display()``` method to draw a circle instead of a rectangle:

```javascript
class MovingCircle extends MovingShape {
    
    constructor(startX, startY, startSize, colour){
        super(startX,startY,startSize);
        this.colour = colour;
    }

    move() {
        super.move();
    
    }

    display() {
        noStroke();
        fill(this.colour);
        ellipse(this.x, this.y,this.size,this.size);
    }

}
```

So, just define a global variable call ```circ```. Now can you remember what you need to do to initialise circ, the move and display it?! HINT take a look towards the end of Task 4... You may need to use the ```color()``` [function built into p5](https://p5js.org/reference/#/p5/color)...

<p align="center">
  <img width="500" height="400" src="./images/capture4.jpg">
</p>

When you get here, come and find me for a digital high five.

Now let's play:

Can you add a way of making a random colour for every object?

Can you add some different shapes?
