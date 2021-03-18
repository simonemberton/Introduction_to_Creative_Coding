# Week 17

## Adding Sound

### Task 0 - Local Server

- You will need to use a local server such as the python SimpleHTTPServer for this tutorial. Read through [this page](https://github.com/processing/p5.js/wiki/Local-server) from the lovely p5 peoples which explains more about local servers.


- Make sure you start your work (make a blank sketch with the p5 complete folder downloaded from p5js.org) on the local disk of the machine you are working on. This means working in the Documents folder in your username path and keeping within your ITCC folder structure. Once you have made a blank sketch in your "Week 17 folder", carry on to the next point:

- If you are happy with the Chrome Extension, just use that and move on to Task 1. Otherwise, if you are on Mac we have some instructions below for using the Mac terminal. If you are on Windows please follow the tutorial on the link above...

- OK, now open the terminal application by hitting cmd+spacebar to open spotlight, then type "terminal" and hit enter.

- In terminal, type the letters "cd" and then spacebar. "cd" means current directory, and you are telling the terminal to "look" at a particular file... Now drag your p5 folder that you just made into the terminal window. You will see that it populates with the file path of the folder.

- Hit enter, you are now telling the terminal application to "look" inside that folder.

- Still in terminal, now type ```python -m SimpleHTTPServer 8000``` and hit enter. You should see a response saying "Serving HTTP on 0.0.0.0 port 8000 ..."

- Now head open Chrome and in the address bar type localhost:8000/empty-example and it will load your page. You may not see anything yet if it's a blank sketch!

- Make sure that when you load your page, you are loading via the localhost address rather than the file path address.

### Task 1 - Soundfile

OK, first things first, let's load a sound file and play it. Make a new blank sketch with required folder structure - how many times have you done this now?! Muscle memory should hopefully be kicking in...

Let's define a variable called ```song``` and a function to preload a .mp3 file. You can find loads of sounds at www.freesound.org. Make sure you save it to the same folder as your sketch.

```javascript
let song;

function preload() {
  song = loadSound("PATH TO YOUR SOUND FILE");  
}

```

Now, let's make a setup function that initialises a bunch of stuff and plays our song. You'll see that we suspend our audioContext, this is just because some browsers require a user gesture to start audio processing.

```javascript
function setup() {
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  createCanvas(480, 270);
  background(0);
  fill(255);
  textAlign(CENTER);
  text("click to play/pause", width/2, height/2);
  song.play();
  noLoop();
}
```

Try running your sketch by opening your index.html page via the localhost:YOUR PORT NUMBER address.

We don't actually need a draw function here.

So, you may see that in the console it is coming up with a warning saying that AudioContext was not allowed to start. Some browsers have been designed to stop audio automatically playing. This is frustrating, but we just need to get around it by adding a mousePressed function (beneath setup()) that starts audio for us:

```javascript
function mousePressed() {
  userStartAudio();
}
```

### Task 2 - Mouse Interaction 1

#### Soundfile

Let's add the ability to pause and play our song based on toggling with a mouse button press.


```javascript

function mousePressed() {
  userStartAudio();
  if (song.isPlaying()) {
    song.stop();
  } else {
    song.play();
  }
}
```

#### Synthesis

With synthsis it's slighty different. We have to make an oscillator and make our own system of checking whether it's playing or not. So we start off with a couple of variables. The let ```playing``` is just going to go between true and false, what's that type of variable called? [HINT](https://en.wikipedia.org/wiki/Boolean_data_type).

Let's make a new blank sketch, call it sketchSynth.js or something. Remember what you have to do in index.html to get this file to load? You will need to change from sketch.js to sketchSynth.js somewhere...

OK, now let's create a couple of global variabls:

```javascript
let osc;
let playing = true;
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

### Task 3 - Mouse Interaction 2

Now let's try mapping some mouse interaction to the frequency of our sound generators.

#### Soundfile

Go back to your original sound file play back sketch. Comment out the ```noLoop()``` line.

Then change the ```song.play()``` to ```song.loop()```;

OK so we're going to map the Y position of the mouse to the playback rate of the audio file. Anything with a negative number will play the sound in reverse, which is pretty cool! Remember that Y=0 is at the top.

```javascript
function draw() {
  let speed = map(mouseY, 0.1, height, 2, -2);
  speed = constrain(speed, -2, 2);
  song.rate(speed);
}
```

#### Synthesis

We can do the same in the draw function of our synthesis patch:

```javascript
function draw() {
  let speed = map(mouseY, 0.1, height, 2000, 100);
  speed = constrain(speed, 100, 2000);
  osc.freq(speed);
}
```


#### Sound Effects

Let's try some reverberation. Reverb is an effect that mimics the way real world sound is reflected off objects and surfaces in the environment.

We need a global variable for our first audio effect, we're going to make a reverb:

```javascript
let reverb;
```

Then, in setup(), try adding this:

```javascript
//other code making oscillator.
reverb = new p5.Reverb();
osc.disconnect(); // so we'll only hear reverb...
reverb.process(osc, 10, 2);
```
Now try the same process for adding a delay. Delay is an echo that repeats the sound using a feedback loop. Have a look [here](https://p5js.org/reference/#/p5.Delay) for how to use it.

### Task 4 - Event Driven Sound

So let's pick up from the session where we created a particle system with forces. 

If you did not complete the task, it is definitely a good idea if you do. But for time's sake we've given you some starter code which is in the learning materials on Blackboard. Using a local server, open the index.html in your browser. Then open sketchStarter.js in your code editor of choice.

[Here](https://davemeckin.panel.uwe.ac.uk/Week_17_Demo) is my version of the final piece. This fits the brief and could be submitted. But obviously we want you to make your own work! (Remember you have to click the mouse to start the audio).

Make sure you start this off only making 5 particles (numParticles = 5), sound is very CPU intensive in the browser!

You should see though, that our particles just float of screen right now. So what we're going to do is constrain their movement and trigger a sound when they hit the edge of the canvas...

#### Synthesis

Let's declare some global variables. We need to create an array some note values; a flag to check whether our mouse has been clicked and some empty variables that we are going to add our audio effects to later on.

```javascript
let scaleArray = ['C4', 'D4', 'E4', 'G4', 'A4', 'C5', 'D5', 'F5', 'G5']; // Array of musical notes 
let clicked = false; //flag to check whether mouse has been clicked
let delay, reverb; // our effects.
```

Now, in our Particle constructor. Let's add some code so that randomly pick a note from the note array. We're going to make a MonoSynth, which is an oscillator whose amplitude is controlled by an envelope, and it's packaged up nicely for us in a single object. 

```javascript
constructor(startX, startY, startMass){
    this.mass = startMass;
    this.r = 10;
    this.pos = createVector(startX, startY);
    this.vel = createVector(random(0.5,2.5), random(0.5,2.5));
    this.acc = createVector(0, 0);
    ///*** new stuff ***///
    this.note = scaleArray[Math.round(random(0, scaleArray.length))]; //randomly pick an element from the note array
    this.synth = new p5.MonoSynth(); // create a new MonoSynth
  }
```
Then in setup we need to update what's in there so we:  

- Add the line to suspend the audio context, because we will need a mousePressed gesture to start the audio afterwards.
- Connect the MonoSynth in each particle to our reverb and delay effects.


```javascript
function setup() {
  createCanvas(windowWidth, windowHeight); // create a canvas that fills the window
  getAudioContext().suspend(); // Ensuring our audio is stopped, before being triggered on mousePress below. Stupid Chrome :)
  delay = new p5.Delay(); // make delay
  reverb = new p5.Reverb(); // make reverb
  for (let i = 0; i < numParticles; i++) {
      particles[i] = new Particle(random(50,width-50),random(50,height-50),random(4,8));

      ////
      delay.process(particles[i].synth, .22, .75, 2900); // hook our particle synth up to the delay
      reverb.process(particles[i].synth, 10, 8); // hook our particle synth up to the reverb
    }

    for (let i = 0; i < 1; i++) {
      attractors[i] = new Attractor(random(0,width),random(0,height));
    }
}
```

And then, just because Google have been annoying, we need to just add a block that allows us to start the audio processing when there is some user interaction. For some reason, Google have made it so that, to access the Web Audio API you need to provide some positive interaction. So let's update our mousePressed function:

```javascript
function mousePressed() {
  userStartAudio();
  }
```


Finally, we just need to update our checkEdges function to trigger our envelope and make a sound when the particle hits the edge of the canvas (and stop them from leaving!):

```javascript
checkEdges() {

    if (this.pos.x > (width-this.r)) {
      this.vel.x *= -1;
      this.pos.x = width-this.r;
      this.synth.play(this.note); // play our note on collision with the outside of the canvas
    } else if (this.pos.x < (0+this.r)) {
      this.vel.x *= -1;
      this.pos.x = 0+this.r;
      this.synth.play(this.note); // play our note on collision with the outside of the canvas
    }

    if (this.pos.y > (height-this.r)) {
      this.vel.y *= -1;
      this.pos.y = height-this.r;
      this.synth.play(this.note); // play our note on collision with the outside of the canvas
    } else if (this.pos.y < (0+this.r)) {
      this.vel.y *= -1;
      this.pos.y = 0+this.r;
      this.synth.play(this.note); // play our note on collision with the outside of the canvas
    }

  }
```



### Task 5 - State Change

One other thing, I want to just show you quickly how to change the state of your piece. This uses an if statement to toggle between two states. But you coud also use a switch statement to cycle between multiple states by using a counter as did in week 15. Let's add a toggle function to invert the colours of the piece and change the waveform of the synths to create a distinct audio-visual difference:

```javascript
function mousePressed() {
  userStartAudio();
   if (!clicked) {
    bgColour = 255;
    particleColour = 0;
    clicked = true;
    for (let i = 0; i < numParticles; i++) {
      particles[i].synth.oscillator.setType("sawtooth"); // Changing the waveform of our synth's oscillator
    }
  } else {
    bgColour = 0;
    particleColour = 255;
    clicked = false;
    for (let i = 0; i < numParticles; i++) {
      particles[i].synth.oscillator.setType("sine"); // Changing the waveform of our synth's oscillator
    }
  }
}

```




### Task 6 - Stretch Goals

Stretch goal: instead of only having 2 states and toggling between them, how can you make more? HINT: use a [switch statement](https://www.w3schools.com/js/js_switch.asp)..

What kind of parameters from the objects in your sketch can you map to sound?

What about:

- Speed to volume? [HINT](https://p5js.org/reference/#/p5.SoundFile/setVolume)
- X Position to pan? [HINT](https://p5js.org/examples/sound-pan-sound.html)
- Y Position to filter frequency? [HINT](https://p5js.org/reference/#/p5.Filter)

etc etc etc

