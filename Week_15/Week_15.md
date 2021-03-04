# Week 15

## A More Complex Game Exercise

This exercise leans heavily on [this tutorial](https://thecodingtrain.com/CodingChallenges/031-flappybird.html) by Dan Shiffman. So credit goes to him for the idea and most of the implementation! Our aim for this session is to demonstrate how we can use simple interaction design (a single spacebar key press) to create an engaging experience with multiple "states". One key syntax difference between how we are working in this tutorial and how Dan Shiffman does it, is that we are using JavaScript Classes, in line with how we've been learning, where Dan Shiffman uses functions.

We're going to create a very simple minimal version of [Flappy Bird](https://flappybird.io/), but our version is going to:

 - Have a health value that decreases when you hit a pipe. 
 - An interaction mechanic changes increases the level of difficulty over time.

[This](https://davemeckin.panel.uwe.ac.uk/Week_15_Demo/) is what we're going to be making. 

For this week's tutorial, we are going to work with some starter code. This is because we want to demonstrate a few particular concepts. If you would like to understand more about how all the code works, please do watch the Shiffman tutorial linked above after you've followed this worksheet. 

Please download and work from the week_15_startercode.zip file in the Blackboard Learning Materials section for this week. Unzip the folder and place it in your Semester 2->Week 15 folder which should be located with all your other Introduction to Creative Coding materials.


### Task 0 - Familiarisation

OK so the folder structure here shouldn't be new to you by now. It's our standard empty example. But you'll see that there are some extra files here. First of all, take a look in index.html and see that we're using the script tags to include the pipe.js and bird.js files.

Now also open the pipe.js and bird.js files. Take a look through the code and see what's going on, can you work out what each of them are going to do? And how we're going to use them? No worries if it's a bit unclear right now, all will be revealed as we get making our game...


### Task 1 - The Birrrd

#### Setting Up

Right, now open our sketch.js file. This is just a blank sketch with our setup() and draw() functions. OK let's get into it and declare all the variables we're going to need for our game. The names should give you a clear idea of what function each variable will perform:

```javascript
let bird;
let pipes = [];
let pipeInterval = 120;
let timer = 0;
let timerInterval = 60;
let level = 0;
let health = 10;
```

Great, now let's update our setup() function to create the canvas; make our bird and add a new pipe into our pipe array. We'll use the pipe array in the next task. We're also going to turn the outline off using noStroke():

```javascript
function setup() {
  createCanvas(windowWidth, windowHeight);
  bird = new Bird();
  pipes.push(new Pipe());
  noStroke();
}
```
Now let's update our draw()function by setting our background colour and added a call to a new function called runGame(). We're going to pass an argument of 1 to that function, this will be used later in Task 5, where we get you to do a bit of thinking yourselves!

```javascript
function draw(){

  background(220);
  runGame(1);

}
```


#### Running Our Game in a Separate Function

If you tried to run the code, you'd notice that we'd get an error because we haven't actually declared our runGame() function yet. So let's do that! In that function for now, we're just going to add calls to the Bird's methods of update() and show(). You'll also notice we've got the argument being passed into runGame called intervalScale which we're going to use in Task 5:

```javascript
function runGame(intervalScale){

  bird.update();
  bird.show();

}
```
#### Interaction

Cool so we've got a bird! But at the moment it just falls to the floor! Not much use, right? So let's add our interaction using the spacebar key. We want to add the keyPressed() function ***below*** the draw() function:

```javascript
function keyPressed() {

  if (keyCode === 32) {
    bird.up();
    //console.log("SPACE");
  }
}
```

Nice, now we should have our minimal Bird that falls to the floor if we don't do anything, but flies up when we press spacebar!

<p align="center">
  <img src="./images/Task01.png">
</p>

### Task 2 - Pipes and Timing

It's not much of a game yet, so let's go ahead and add some obstacles. We're using an array of pipes, so we want to be **iterating** through that array using a for loop. And one important thing to note is that we always want to be iterating from the end of the array down to the beginning, because we're going to be adding stuff to the end first by using push() later on. So, let's update our runGame() function to iterate over the pipes array and call both the update() and show() functions:

```javascript
function runGame(intervalScale){

  for (let i = pipes.length-1; i >= 0; i--) {
    pipes[i].show();
    pipes[i].update();
  }

  bird.update();
  bird.show();

 
}
```

Right, now let's test for whether our Bird collides with a pipe. All the collision testing code is in the pipe.js file. It's pretty similar to what we were doing last week but because there are two sections to the pipe, we have to nest some if statements:

This makes it easy for us to now call the hits() method of each pipe. We're going to do this still in the for loop:

```javascript
 if (pipes[i].hits(bird)) {
      console.log("HIT");
    }
```

Still in the for loop, let's remove pipe from our pipes array if not on screen anymore using the splice method:

```javascript
if (pipes[i].offscreen()) {
      pipes.splice(i, 1);
    }
```

Now, we're moving our of that for loop and at the bottom of runGame(), let's add a bit of timing code in the same manner as we did last week, for adding a new pipe at a given interval: 

```javascript
 if (frameCount % pipeInterval === 0) {
            pipes.push(new Pipe());
  }
```

Hopefully those steps were clear and you've written all that code out, but just in case you want to check, take a look at what runGame() is looking like:

<details>
<summary>Want to see the code?</summary>
<br>
This is what our runGame() function looks like now:
<br><br>
<p>
  
```javascript
function runGame(intervalScale){

  for (let i = pipes.length-1; i >= 0; i--) {
    pipes[i].show();
    pipes[i].update();

    if (pipes[i].hits(bird)) {
      console.log("HIT");
    }

    if (pipes[i].offscreen()) {
      pipes.splice(i, 1);
    }
  }

  bird.update();
  bird.show();

  if (frameCount % pipeInterval == 0) {
            pipes.push(new Pipe());
  }
}
```
</p>
</details>

Nice, so we should have some pipes that turn red if we collide with them now!

<p align="center">
  <img src="./images/Task02.png">
</p>

### Task 3 - Creating Our State Machine

OK now we're going to update our draw() function such that our game will operate differently, depending on what level we're on. [here](https://www.youtube.com/watch?v=-Yicg2TTMPs) is a nice little explanation of what we're creating using an oven as an example. Our state machine is going to have 5 states based on what our level variable is doing:

- Game Over
- Splash Screen
- Level 1
- Level 2
- Level 3

So let's use a switch case statement to implement those states in our draw() function:

```javascript
function draw() {
  background(220);
  switch(level) {
    case -1:
        textSize(120);
        fill(255,0,0);
        textAlign(CENTER, CENTER);
        text("GAME OVER! \n Press Enter to Start", width/2, height/2);

    break;

    case 0:
        textSize(100);
        fill("#0f82af");
        textAlign(CENTER, CENTER);
        text("Press Enter to Start", width/2, height/2);
        //console.log("enter screen");
    break;

    case 1:
        runGame(1);
         
    break;

    case 2:
        runGame(1);
          
    break;
    
     case 3:
        runGame(1);
         
    break;
  }
 
}
```

You'll notice that all three of our levels currently have the same argument being passed to our runGame() function. This is because we're going to get you to do some logical thinking later on to update these values...

Great, but now our game doesn't actually function because all that happens is that we enter our splash screen and don't go anywhere! So let's update our keyPressed function to listen out for the enter key and to reset all necessary values for beginning our game:

```javascript
function keyPressed() {

  if (keyCode === ENTER) {
    level = 1;
    health = 10;
    pipes = [];
    timer = 0;
    console.log("START");
  }

  if (keyCode === 32) {
    bird.up();
    //console.log("SPACE");
  }
}
```

<p align="center">
  <img  src="./images/Task03.png">
</p>

Nice, now we're cooking...

### Task 4 - Changing Level

We do now need to add the mechanics for changing our levels though.

So let's think about our main timer. We want to use the same code as we did to time adding new pipes. So, **at the very bottom of runGame()**, let's add the following:


```javascript
if (frameCount % timerInterval == 0) {
    timer ++;
    //console.log(timer);
  }
```

We also need to think about adding some feedback to our players, so they can know how they're doing on the game. So let's add this, just like we had last week. Again we're doing this at the very bottom of runGame() because we want to draw this last so that our pipes don't obscure it:

```javascript
 textSize(30);
    fill("#0f82af");
    textAlign(LEFT, CENTER);
    text("Health: " + nf(health, 1, 2), 10, 30);
    text("Level: " + level, 200, 30);
```
Let's make sure our health gets changed when we collide, so update our collision condition **in the for loop in runGame()** to reduce health by 0.1 each frame we're colliding:

```javascript
if (pipes[i].hits(bird)) {
      console.log("HIT");
      health -= 0.1;
    }
```

Cool, so you should be seeing the health now reducing every time you hit a pipe.

But we still haven't started changing our levels based on the timer yet. 

We're going to write our own function for that. So first of all, let's add the call to a function called testLevel() **at the top of runGame()**:

```javascript
testLevel();
```


#### Challenge:
Set our timer ranges in order to select which level we are on


* If our health is above 0
   - Level 1 if timer is between 0 and 20
   - Level 2 if timer is greater than 20 and less than or equal to 40
   - Level 3 if timer is greater than 40 

<details>
<summary>Want to see the code?</summary>
<br>
This is what our testLevel() function looks like :
<br><br>
<p>
  
```javascript
function testLevel(){
  if(health <= 0) {
      level = -1;
    } else {
       if(timer <= 20) {
          level = 1;
        } else if (timer > 20 && timer <= 40) {
          level = 2;
        } else if (timer > 40) {
          level = 3;
        }
    }
}
```
</p>
</details>


So now we have some pretty complex code in our runGame() and testLevel() functions:


<details>
<summary>Want to see the code?</summary>
<br>
This is what our runGame() and testLevel() functions look like now:
<br><br>
<p>
  
```javascript
function runGame(intervalScale){
    testLevel();

  for (let i = pipes.length-1; i >= 0; i--) {
    pipes[i].show();
    pipes[i].update();

    if (pipes[i].hits(bird)) {
      console.log("HIT");
      health -= 0.1;
    }

    if (pipes[i].offscreen()) {
      pipes.splice(i, 1);
    }
  }

  bird.update();
  bird.show();

  if (frameCount % pipeInterval == 0) {
            pipes.push(new Pipe());
  }

  if (frameCount % timerInterval == 0) {
    timer ++;
    //console.log(timer);
  }

    textSize(30);
    fill("#0f82af");
    textAlign(LEFT, CENTER);
    text("Health: " + nf(health, 1, 2), 10, 30);
    text("Level: " + level, 200, 30);
     

}

function testLevel(){
  if(health <= 0) {
      level = -1;
    } else {
       if(timer <= 10) {
          level = 1;
        } else if (timer > 20 && timer <= 40) {
          level = 2;
        } else if (timer > 40) {
          level = 3;
        }
    }
}
```
</p>
</details>


<p align="center">
  <img  src="./images/Task04_01.png">
</p>

And:

<p align="center">
  <img  src="./images/Task04_02.png">
</p>

### Task 5 - Increasing Difficulty Based on the Timer


Now we're going to use the intervalScale argument being passed to runGame() to increase the difficulty. What we're doing here is reducing the interval at which a new pipe is drawn, thus making it harder:

```javascript
if (frameCount % (pipeInterval*intervalScale) == 0) {
            pipes.push(new Pipe());
  }
```

OK, now what do you have to do to the different calls to runGame() in the switch case statement in the draw() function? HINT it should be less than the number we currently have for level 1, in levels 2 and 3...

Nice one! Now you've got a fairly complex game with timing and dexterity mechanics as well as levels!

### Task 6 - Stretch Goals

- Can you change the shapes being drawn?
- Can you make the game harder by increasing the velocity at which the pipes are moving depending on the level?
- Can you make the game harder by increasing the amount of gravity or decreasing the amount of lift acting upon the Bird?
- How can you really make this game your own in terms of colour scheme and fonts etc?
- Can you use the microphone input to control the bird's flight?

