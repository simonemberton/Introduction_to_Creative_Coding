# Week 19

## Adding Sound

### Task 0 - Using the P5 editor

- You will need to use the P5 editor for this tutorial.  [Here](https://editor.p5js.org/) 


- Make sure you log in (make an account)

- create a ```console.log()``` statement in ```setup()``` to check everything is working.


### Task 1 - Soundfile

***Make sure the link to the p5.sound library is uncommented in your index.html***

OK, first things first, let's load a sound file and play it. Make a new blank sketch with required folder structure - how many times have you done this now?! Muscle memory should hopefully be kicking in...

Find and upload a sound file to your p5 sketch. You can find loads of mp3 sounds at www.freesound.org (make an account). Download on and upload it to your sketch. 

![alt text](https://nycdoe-cs4all.github.io/images/lessons/unit_3/3.1/image1.gif "upload file")  

Define a variable called ```song``` and a function to preload a .mp3 file. 

```javascript
let song;

function preload() {
  song = loadSound("PATH TO YOUR SOUND FILE");  
}

```

Now, let's make a setup function that initialises the audio. You'll see that we suspend our audioContext, this is just because most browsers require a user gesture ('click') to start audio processing.

```javascript
function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(480, 270);
  background(0);
  fill(255);
  textAlign(CENTER);
  text("click canvas to play", width/2, height/2);
  noLoop();
}
```

We just need to get around it by adding a mousePressed function (beneath setup()) that starts audio for us:

```javascript
function mousePressed() {
  userStartAudio();
  song.play();
}
```

We don't actually need a draw function here. You can comment it out.


### Task 2 - Mouse Interactions

Try to get your sound to start and stop playing based on a mouse click...

*****
### &#x1F536; Task 2 first code challenge:  

```diff
! Edit the mousePressed function in sketch.js  
! to pause and play the song based on clicking with a mouse button press.
``` 

### Task 3 - More Mouse Interactions

Remove the text and add a circle to your ```setup()``` function along with some variable for the x-coordinate of the center of the circle, the y-coordinate of the center of the circle and the diameter of the circle.  

sketch.js should like this:

```javascript
let song;
let circleX = 130; // x-coordinate of the center of the circle.
let circleY = 30; // y-coordinate of the center of the circle.
let circleD = 120;// Number: diameter of the circle.

function preload() {
  song = loadSound("piano.mp3");  
}

function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(480, 270);
  background(0);
  fill(255);
  circle(circleX, circleY, circleD);
  noLoop();
  console.log("hi");
}

function mousePressed() {
  userStartAudio();
  if (song.isPlaying()) {
    song.stop();
  } else {
    song.play();
  }
}
```


*****
### &#x1F536; Task 3 - events - second code challenge:  

```diff
! Use the dist() function in p5 (look up in the reference)  
! To calculate the distance between the mouse click and the centre of the circle
! Stop / start the audio if the mouse click is within the circle (using it like a play button).
``` 
### Task 4 - Even More Mouse Interactions  

Now we're going to map the Y position of the mouse to the playback rate of the audio file. Anything with a negative number will play the sound in reverse. Remember that Y=0 is at the top.

In your sound file play sound sketch. Comment out the ```noLoop()``` line.

Then change the ```song.play()``` to ```song.loop()```;
 

```javascript
function draw() {
  let soundSpeed = map(mouseY, 0.1, height, 2, -2);
  soundSpeed = constrain(soundSpeed, -2, 2);
  song.rate(soundSpeed);
}
```

### Task 5 - Synthesis

With synthsis it's slighty different. We have to make an oscillator and make our own system of checking whether it's playing or not. So we start off with a couple of variables. The let ```playing``` is just going to go between true and false, what's that type of variable called? [HINT](https://en.wikipedia.org/wiki/Boolean_data_type).

Let's make a new blank sketch, and sketch.js. 

Create two global variabls:

```javascript
let osc;
let playing = false;
```

Next in our setup function let's make our oscillator and start it with a frequency of concert pitch A:

```javascript
function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(200, 200);
  osc = new p5.Oscillator(); //create our oscillator
  osc.setType('sine'); // try other waveforms such as 'sawtooth' 'square' 'triangle'
  osc.freq(440); // set the frequency to be 440 Hz or concert pitch A
  osc.start(); // start our oscillator making sound
}
```

And finally let's toggle between fading the oscillator in and out. We have to use our ```playing``` flag to check if it is playing and act accordingly.

```javascript
function mousePressed() {
  userStartAudio();
  if (mouseX > 0 && mouseX < width && mouseY < height && mouseY > 0) { //check if we're in the canvas
    if (!playing) {// what does the ! operator mean?
      // ramp amplitude to 0.5 over 0.5 seconds
      osc.amp(0.5, 0.5);
      playing = true;
    } else {
      // ramp amplitude to 0 over 0.5 seconds
      osc.amp(0, 0.5);
      playing = false;
    }
  }
}
```

What happens if you want to change the tone of the oscillator? How would you do that? [HINT](https://p5js.org/reference/#/p5.Oscillator/setType)

### Task 6 - Mouse Interaction 

Now let's try mapping some mouse interaction to the frequency of our sound generators.

Like before we're going to map the Y position of the mouse to the playback rate of the audio file.

We can do the same in the draw function of our synthesis patch:

```javascript
function draw() {
  let soundSpeed = map(mouseY, 0.1, height, 2000, 100);
  soundSpeed = constrain(soundSpeed, 100, 2000);
  osc.freq(soundSpeed);
}
```

*****
### &#x1F536; Task 6 - Mouse Interaction  - third code challenge:  

```diff
! Create 2 Rectangles in a new sketch 
! Use a mouse press to trigger different sounds and alter their speed or frequency.
! The sounds could be audio files or different generated frequencies / waveforms: 
! eg 'sawtooth' 'square' 'triangle'.
``` 

