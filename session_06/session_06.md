# Session 06

## Debugging and problem solving

### Task 1 - Tutorial on debugging

Read the following tutorial on debugging [HERE](http://staging.p5js.org/tutorials/debugging.html).

### Task 2 - JavaScript console

You should now be pretty familiar with looking for errors in the console if something isn't working.  You can actually do some other cool things with the console.  Let's have a look.

Run the following code

```javascript

var x = 0;

function setup() {
  createCanvas(400, 400);
  noFill()
  stroke(255);
}

function draw() {
    background(0);
    ellipse(x, height/2, 50);

    x = x + 1;
    if (x > width) {
      x = 0;
    }
    
    console.log(x);
}

```

Here we're moving an ellipse across the canvas horizontally.  If you look at the console you can see the x location for each frame.  Comment out the ```console.log()``` function and we'll try writing some commands directly into the console.  Notice if you just write ```x``` and click return you get the value of ```x``` in real time.  You can also change the value of the variable, try writing ```x = 50;``` and see how it jumps to that location.  You can stop the draw function all together with the ```noLoop()``` function and start it again with the ```loop()``` function.  Okay enough of that now let's try and fix some errors!

### Task 3 - Debugging challenge

Look at the following code

```javascript

var bugs = []; // array of Bug objects

function setup() {
  createcanvas(710, 400);
  noFill()
  Stroke(255);
  // Create objects
  for (var i = 0; i < 50; i++) {
    bugs.push(new Bug(random(w), random(h), random(10, 30)));
  }
}

// create new object when mouse is pressed
function mousepressed() {
  var r = random(10, 30)
  var b = new Bug(mouseX, mouseY, r);
  bugs.push(b);
}

function draw() {
  background(0);
  // move and display all the objects
  for (var i = 0; i < bugs.length; i++) {
    bugs[i].move();
    bugs[i].display();
  }
}

// Bug class
function Bug(x_, y_, r_) {
  this.x = x;
  this.y = y;
  this.diameter = r;
  this.speed = 2;

  this.move = function() {
    this.x += random(-this.speed, this.speed);
    this.y += random(-this.speed, this.speed)
  };

  this.display = function() {
    ellipse(this.x, this.y, this.diameter, this.diameter);
  };
}

```

If you try running this code you'll see that it was written in a hurry and is littered with errors.  Your task is to use your programming knowledge and debugging skills to find and fix all the errors in the code so that is runs successfully.  When finished you should be able to create a new object each time you press the mouse.

### Task 4 - Problem solving challenge

Making use of the following code

```javascript
 
function setup(){
    console.log(LetterCapitalize("hello world"));
}

function LetterCapitalize(str) { 

    // code goes here  
    
    return str;      
}
```
   
your challange is to write some more code inside the ```LetterCapitalize(str)``` function that takes the ```str``` parameter being passed and capitalises the first letter of each word.  For example if the input is "hello world" the output should be "Hello World".  You can assume that words will be separated by only one space.
