
# Week 18

## Audio reactive visuals

For this week's task we're going to make an interactive audio-visualiser.  This worksheet is inspired by Yannis Yannakopoulos' [audio-visualisers](https://tympanus.net/Development/AudioVisualizers) so spend a bit of time playing with them first.  You'll notice that the location of the mouse cursor on the canvas allows you to interact with them, we'll also see how we can use input from the microphone and camera as other ways to interact with our audio-visualisers.

### Task 1 - Microphone input

If you are using a recent version of p5 you'll need to run a local server for this worksheet.  If you've forgotten how to do that you can check [here](https://github.com/processing/p5.js/wiki/Local-server).  When using a local server you may find that any changes you mkae to the code do not show up in the browser, this may be due to the browser cache and can be remedied with a hard refresh (Cmd+Shift+R on a Mac, and Ctrl+F5 on a PC).

Have a look at the [`p5.AudioIn`](https://archive/p5js.org/reference/#/p5.AudioIn) example and get it working on your machine. You might want to make the canvas bigger as on this example it's really tiny. At the time of writing there was an issue with `p5.AudioIn` (in the latest version of p5.sound) and Firefox, so please use Chrome or Safari.

Notice in this example you are required to press the mouse cursor on the canvas before it starts working.  If you are using Chrome you'll notice there is a warning in the console about the AudioContext not being allowed to start.  This is to force developers to include a play button or such like so that users can choose to play a sound rather than it just blasting as soon as you open a webpage.  You can read more about it [here](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes#webaudio).  One way to get around this is to call the [`userStartAudio()`](https://p5js.org/reference/#/p5/userStartAudio) function inside the `setup()` function.  In this example pressing the mouse cursor on the canvas calls `userStartAudio()`.

If you don't have a microphone on your computer you could instead just use an audio track for the audio input.  If this is what you're doing you can start from the code of this [`getLevel()` example](https://p5js.org/reference/#/p5.Amplitude/getLevel).

I'm going to base my audio-visualiser off of [Demo 3](https://tympanus.net/Development/AudioVisualizers/index3.html).  This example consists on rotating points and lines that move in time to the music.  

Notice that they all rotate around the centre of the canvas, so we need to change our point of origin using the `translate()` function.  Now let's use a for loop and the `rotate()` function to draw some [points](https://p5js.org/reference/#/p5/point) in a circle shape.  This is the code that I added to my `draw() function`:

```javascript
translate(width / 2, height / 2);

let noOfPoints = 12;

for (let i = 0; i < noOfPoints; i++) {
  rotate(TWO_PI / noOfPoints);

  strokeWeight(4);
  stroke(255);
  point(0, height/4);
}
```

Now I want to use the values coming in from the micrphone to move the points.  If you look at the code you'll see that the `getLevel()` function is being used to access the volume level (amplitude) coming into the microphone.  `console.log()` the values in the `micLevel` variable to the console while making some noises and see how they change.  Note that you'll have to tap on the canvas to start the audio before you see these values.  In particular look for the extremes. What are the values when there is only background ambience? What are the values when you make a loud clapping noise?

Next you'll need to use the `map()` function to map the minimum and maximum values from the microphone to the minimum and maximum values that you want the points to move to.

Your `draw()` function should now look something like the following:

```javascript
function draw(){
  background(0);
  fill(255);
  noStroke(); // so that the text below doesn't have a large stroke value
  text('tap to start', width/2, 20);

  let micLevel = mic.getLevel();
  let mappedMicLevel = map(micLevel, 0, 0.1, 0, 200); // map micLevel to desirable range

  translate(width / 2, height / 2); // move point of origin to centre of canvas

  let noOfPoints = 12; // total number of points to draw

  for (let i = 0; i < noOfPoints; i++) { // for all of the points
    rotate(TWO_PI / noOfPoints); // rotate around a circle - default for p5 is radians

    strokeWeight(4);
    stroke(255);
    point(mappedMicLevel, height/4);
  }

}
```

Now put some music on and watch those points dance!  You might need to adjust the values inside the map function depending on how loud your music is.  

Here is a screenshot of my end of Task 1 and a [link](https://simonemberton.panel.uwe.ac.uk/Week18/Task1/) to see it working.

<p align="center">
  <img width="499" height="498" src="./images/Task_1.gif">
</p>


### Task 2 - Frequency Analysis

Let's try and develop this a bit further by applying some audio analysis to the incoming signal so that we can isolate the high, mid and low frequency bands rather than just overall amplitude.  The method of audio analysis we'll be using is the Fast Fourier Transform [FFT](https://p5js.org/reference/#/p5.FFT).  

First we need to create a new global variable called `fft` at the top of the `sketch.js` file.  Remember it's a global variable so it's outside of the `setup()` function.  Next at the end of the `setup()` function we'll make our `fft` variable equal to a new `p5.FFT` object and then set the input to `fft` to be the `mic` variable from the `p5.AudioIn` object using the following code:
```javascript
fft = new p5.FFT();
fft.setInput(mic);
```
Then inside the `draw()` function instead of calling `mic.getLevel()` we'll use the following code to get the frequency spectrum of the microphone signal:
```javascript
let spectrum = fft.analyze();
```
Now `console.log` the `spectrum` variable and see the output in the console. You'll need to comment out the rest of the code so that errors don't prevent the code from running.  You'll see in the console that the `spectrum` variable contains an array of length 1024 where each location in the array has a value between 0-255 which represents the amplitude at that slice of the frequency spectrum.

We could use the data from `fft.analyze()` to create some interesting visuals but instead I want to make use of fft's [`getEnergy()`](https://p5js.org/reference/#/p5.FFT/getEnergy) function to get the average values from predefined frequencies ranges, in particular the treble, mid and bass frequencies.  FYI we always have to use `fft.analyze()` before using `fft.getEnergy()`.  Add the following code:
```javascript
let treble = fft.getEnergy("treble");
let mid = fft.getEnergy("mid");
let bass = fft.getEnergy("bass");
```
Now print the values from the `treble`, `mid` and `bass` variables to the console. You'll see that we now have a value between 0-255 for each of the three frequency bands.  Let's draw circles of points for each of these frequency bands.  We need to use `map()` functions for each of the bands to map the values within appropriate ranges.  Here are what my `map()` functions look like:
```javascript
let mappedTreble = map(treble, 0, 50, 0, 200); 
let mappedMid = map(mid, 0, 255, -100, 100); 
let mappedBass = map(bass, 0, 255, -200, 0);
```
Next inside the for loop add more `point()` functions so that we've got one for each frequency band, where the `x` location of each `point()` takes a mapped frequency band as the input. I've made mine a different colour too so that you can see the difference:
```javascript
// treble
strokeWeight(6);
stroke(255,0,0);
point(mappedTreble, height/4);

// mid
strokeWeight(4);
stroke(0,0,255);
point(mappedMid, height/4);

// bass
strokeWeight(6);
stroke(255);
point(mappedBass, height/4);
```
Now you should be able to see each of the points reacting slightly differently to different parts of the music.  If you can't see all three points try adjusting the map function values so that it works well for you.

Let's make those differences a bit more pronounced by adding a scaling factor to each of the circles of points.  We'll create some more `map()` functions so that we have a bit more control over the exact values to use.
```javascript
let scaleTreble = map(treble, 0, 50, 0.8, 1.2); 
let scaleMid = map(mid, 0, 255, -0.9, 0.9); 
let scaleBass = map(bass, 0, 255, -1, 1);
```

We'll then use these mapped scaling variables as input to the [`scale()`](https://p5js.org/reference/#/p5/scale) function for each of the circles of points.  This function scales from the point of origin, so moves the points further from the centre.  We need to isolate the scaling and point functions for each of the frequency bands so that they don't interfere with each other, for that we'll use `push()` and `pop()` to make the drawing of each circle of points self contained.  For example:
```javascript
// treble
push();
  strokeWeight(2);
  stroke(255,0,0);
  scale(scaleTreble);
  point(mappedTreble, height/4);
pop();

// mid
push();
  strokeWeight(3);
  stroke(0,0,255);
  scale(scaleMid);
  point(mappedMid, height/4);
pop();

// bass
push();
  strokeWeight(4);
  stroke(255);
  scale(scaleBass);
  point(mappedBass, height/4);
pop();
```
Now we're getting somewhere.

Here is a screenshot of my end of Task 2 and a [link](https://simonemberton.panel.uwe.ac.uk/Week18/Task2/) to see it working.

<p align="center">
  <img width="499" height="498" src="./images/Task_2.gif">
</p>

### Task 3 - Finishing touches

The audio-visualiser is nearly there but I want to add a few finishing touches as it still feels a bit static.  

First I'm going to add rotations to each of the frequency band shapes which uses the constantly increasing `frameCount` value to drive the rotation.  Then I'll use values from the scaled frequency bands to modify the rotation speed of the shapes.  I'm also going to use values directly from frequency bands to change the colour of the points.  For this I've taken the value from 255 so that the value stays high as the default.

I'm also going to add another shape to the scene for the mid frequencies.  In my example I've used it to draw lines but you could use other shapes here. I want to draw my line from around the centre to the edge of the canvas so I'll create a `map` function which maps the mid frequencies to within that range using the following code:
```javascript
let scaleMidLine = map(mid, 0, 255, 0, width); 
```

The code for drawing the shapes now looks like this:
```javascript
// treble
push();
  strokeWeight(6);
  stroke(255-treble,0,0);
  scale(scaleTreble);
  rotate(-frameCount * scaleTreble/100);
  point(mappedTreble, height/4);
pop();

// mid
push();
  strokeWeight(4);
  stroke(0,0,255-mid);
  scale(scaleMid);
  rotate(frameCount * scaleMid/100);
  point(mappedMid, height/4);
  strokeWeight(2);
  line(0, height/4, scaleMidLine, height);
pop();

// bass
push();
  strokeWeight(6);
  stroke(255-bass);
  scale(scaleBass);
  rotate(-frameCount * scaleBass/100);
  point(mappedBass, height/4);
pop();
```

Finally I'm adding slight trails to the whole visualisation by putting some transparency on the `background()` function at the top of `draw()` like this:
```javascript
background(0,150);
```

Here is a screenshot of the end of Task 3 and a [link](https://simonemberton.panel.uwe.ac.uk/Week18/Task3/) to see it working.

<p align="center">
  <img width="496" height="497" src="./images/Task_3.gif">
</p>
