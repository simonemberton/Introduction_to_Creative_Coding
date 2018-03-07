# Session 11

## Adding Sound

You will need to use a local server such as Mongoose, XAMPP or MAMP for this tutorial. Make sure that when you load your page, you are loading via the localhost address rather than the file path address.

### Task 1 - Soundfile

OK, first things first, let's load a sound file and play it. Make a new blank sketch with required folder structure - how many times have you done this now?! Muscle memory should hopefully be kicking in...

Let's define a variable called ```song``` and a function to preload a .mp3 file. You can find loads of sounds at www.freesound.org. Make sure you save it to the same folder as your sketch.

```javascript
var song;

function preload() {
  song = loadSound("PATH TO YOUR SOUND FILE");  
}

```

Now, let's make a setup function that initialises a bunch of stuff and plays our song.

```javascript
function setup() {
  createCanvas(480, 270);
  background(0);
  fill(255);
  textAlign(CENTER);
  text("click to play/pause", width/2, height/2);
  song.play();
  noLoop();
}
```

Try running your sketch by opening your index.html page via the localhost:YOUR PORT NUMER address.

We don't actually need a draw function here.

### Task 2 - Mouse Interaction 1

#### Soundfile

Let's add the abilitu to pause and play our song based on toggling with a mouse click.


```javascript

function mousePressed() {
  if (song.isPlaying()) {
    song.stop();
  } else {
    song.play();
  }
}
```

#### Synthesis

With synthsis it's slighty different. We have to make an oscillator and make our own system of checking whether it's playing or not. So we start off with a couple of variables. The var ```playing``` is just going to go between true and false, what's that type of variable called? [HINT](https://en.wikipedia.org/wiki/Boolean_data_type).

Let's make a new sketch, call it sketchSynth.js or something. Remember what you have to do in index.html to get this file to load?

```javascript
var osc;
var playing = true;
```

Next in our setup function let's make our oscillator and start it with a frequency of concert pitch A:

```javascript
function setup() {
  createCanvas(200, 200);
  osc = new p5.Oscillator();
  osc.setType('sine');
  osc.freq(440);
  osc.start();
}
```

And finally let's toggle between fading the oscillator in and out. We have to use our ```playing``` flag to check if it is playing and act accordingly.

```javascript
function mouseClicked() {
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

OK so we're going to map the Y position of the mouse to the playback rate of the audio file. Anything with a negative number will play the sound in reverse, which is pretty cool! Remember that Y= 0 is at the 

```javascript
function draw() {
  var speed = map(mouseY, 0.1, height, 2, -2);
  speed = constrain(speed, -2, 2);
  song.rate(speed);
}
```

#### Synthesis

We can do the same in the draw function of our synthesis patch:

```javascript
function draw() {
  var speed = map(mouseY, 0.1, height, 2000, 100);
  speed = constrain(speed, 100, 2000);
  osc.freq(speed);
}
```


#### Sound Effects

Let's try some reverberation. Reverb is an effect that mimics the way real world sound is reflected of objects and surfaces in the environment.

We need a global variable for our first audio effect, we're going to make a reverb:

```javascript
var reverb;
```

Then, in setup(), try adding this:

```javascript
//other code making oscillator.
reverb = new p5.Reverb();
osc.disconnect(); // so we'll only hear reverb...
reverb.process(osc, 10, 2);
```


### Task 4 - Event Driven Sound

So let's pick up from our last session where we created a [particle system with forces](../session_10/session_10.md). Make sure you start this off only making 5 particles, sound is very CPU intensive in the browser!

#### Synthesis

Let's declare some global variables. We need to create an array some MIDI notes; an array of strings with different waveforms that our oscillator can make; then some initial colours and a flag to check whether our mouse has been clicked.

```javascript
var scaleArray = [60, 62, 64, 67, 71, 72, 74, 76, 77]; //array of MIDI note numbers
var waveArray = ['sine','square','sawtooth','triangle']; //sound wave sources
var bgColour = 0; //inital background colour
var particleColour = 255; //inital particle colour
var clicked = false; //flag to check whether mouse has been clicked

var delay, reverb; // our effects.
```

In our particle constructor. We're going to make an oscillator that is controlled by an envelope.

```javascript
constructor(startX, startY, startMass){
    this.mass = startMass;
    this.r = 10;
    this.pos = createVector(startX, startY);
    this.vel = createVector(random(0.5,2.5), random(0.5,2.5));
    this.acc = createVector(0, 0);
    /// new stuff
    this.osc =  new p5.Oscillator(waveArray[Math.round(random(0, waveArray.length))]); //make a new oscillator with a random waveform type
    this.envelope = new p5.Env(); // make a new envelope
    this.envelope.setADSR(0.001, 0.5, 0.05, 0.9); // set attackTime, decayTime, sustainLevel, releaseTime
    this.note = Math.round(random(0, scaleArray.length)); //select a random MIDI note from our scaleArray
    this.envelope.setRange(0.5, 0); //set volume range on the envelope
    this.osc.amp(this.envelope); //map amplitude of envelope to the oscillator
    this.freqValue = midiToFreq(scaleArray[this.note]); // convert our MIDI note to a frequency value for the oscillator
    this.osc.freq(this.freqValue); //set the oscillator frequency
    this.osc.start(); 
  }
```
Then in setup we need to update what's in there:

```javascript
function setup() {
  createCanvas(windowWidth, windowHeight); // create a canvas that fills the window
  delay = new p5.Delay(); // make delay
  reverb = new p5.Reverb(); // make reverb
  for (let i = 0; i < 10; i++) {
      particles[i] = new Particle(random(50,width-50),random(50,height-50),random(4,8));

      ////
      delay.process(particles[i].osc, .22, .75, 2900); // hook our particle oscillator up to the delay
      reverb.process(particles[i].osc, 10, 8); // hook our particle oscillator up to the reverb
    }

    for (let i = 0; i < 1; i++) {
      attractors[i] = new Attractor(random(0,width),random(0,height));
    }
}
```
Finally, we just need to update our checkEdges function to trigger our envelope and make a sound when the particle hits the edge of the canvas:

```javascript
checkEdges() {

    if (this.pos.x > (width-this.r)) {
      this.vel.x *= -1;
      this.pos.x = width-this.r;
      this.envelope.play(this.osc, 0, 0.1);
    } else if (this.pos.x < (0+this.r)) {
      this.vel.x *= -1;
      this.pos.x = 0+this.r;
      this.envelope.play(this.osc, 0, 0.1);
    }

    if (this.pos.y > (height-this.r)) {
      this.vel.y *= -1;
      this.pos.y = height-this.r;
      this.envelope.play(this.osc, 0, 0.1);
    } else if (this.pos.y < (0+this.r)) {
      this.vel.y *= -1;
      this.pos.y = 0+this.r;
      this.envelope.play(this.osc, 0, 0.1);
    }

  }
```



### Task 5 - State Change

One other thing, I want to just show you quickly how to change the state of your piece. Let's add a toggle function to invert the colours of the piece:

```javascript
function mousePressed() {
   if (!clicked) {
    bgColour = 255;
    particleColour = 0;
    clicked = true;
  } else {
    bgColour = 0;
    particleColour = 255;
    clicked = false;
  }
}

```
Now we just need to set our particile colour in the display function of the particle:

```javascript
display() {
    stroke(particleColour);
    strokeWeight(2);
    noFill();
    ellipse(this.pos.x, this.pos.y,this.mass*2,this.mass*2);
  }
```

[Here](http://davemeckin.panel.uwe.ac.uk/Week_18_Demo/sketch_folder/) is my version of the final piece. This could be submitted to fit the brief...

### Task 6 - Parameter Mapping

What kind of parameters from the objects in your sketch can you map to sound?

What about:

- Speed to volume? [HINT](https://p5js.org/reference/#/p5.SoundFile/setVolume)
- X Position to pan? [HINT](https://p5js.org/examples/sound-pan-sound.html)
- Y Position to filter frequency? [HINT](https://p5js.org/reference/#/p5.Filter)

etc etc etc

