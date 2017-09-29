# Session 01

## Anatomy of a Sketch


### Task 0 - Download the p5.zip folder on the link p5complete.zip from p5js.org

- Hopefully you already did this last week! 

- Then copy the folder called "empty-example" in the same enclosing folder, rename it to something like "tutorial-1".

- Go into your newly copied folder and open both index.html and sketch.js in a text editor. Brackets or SublimeText are recommended


### Task 1 - Ready, setup(); and draw();

#### As discussed in our lecture, what are the two most basic building block functions to get our p5 sketch going?

That's right, it's our old friends:

```javascript
function setup(){

}


function draw() {

}
```


#### What do we need to draw on? 

Paper? A wall? Or perhaps a canvas:

```javascript
createCanvas(800,600);
```

Where do you need to put this createCanvas function?

What do the values 800 and 600 relate to?

FYI if you don't define this exlicitly, a default canvas will be made with size 100 x 100

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

This is your first computer program, nice one!

What do the parameters being passed to the ```text()``` function relate to? Hint: [this](https://p5js.org/reference/#/p5/text) will help you learn more...

Now we need to add a circle:

```javascript
ellipse(50,50,40,40);
```


### Task 2 - All the Shapes, well, quite a few anyway...




### Task 3 - Variables




### Task 4 - Operators (+ some maths)




### Task 5 - Make it Functional



### Task 6 - Repetition



### Task 7 - Play












