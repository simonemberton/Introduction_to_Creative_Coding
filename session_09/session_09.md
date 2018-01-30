# Session 09

## Interaction beyond keyboard and mouse 


### Task 1 - Create audio-responsive visualisation

Have a look at the [`p5.AudioIn`](https://p5js.org/reference/#/p5.AudioIn) example and get it working on your machine.

In this example the `getLevel()` function is being used to access the volume or amplitude coming into the microphone.  Print these values to the console while making some noises.  Can you see how they change?  

Now draw a shape to the screen and change the size of the shape depending on the level of amplitude coming in from the microphone.

### Task 2 - Clap detection

We now want to detect if someone makes a sound louder than a clap.  Have a look at the values that are printed to the console when you clap.  After consulting these values choose a threshold value which is lower than the clap value but higher than background noise.  

If the amplitude is over the threshold change a visual element on the page e.g. change the background colour.  What kind of programming statement that you learnt last term might be useful here? 

### Task 3 - Camera input
Now we are going to input some live data from a camera.  Check out the [Video Capture](https://p5js.org/examples/dom-video-capture.html) example.

Notice you have two video images on the screen.  One is the DOM element and one is the image that you're drawing in the `draw()` function.  Try commenting out the `capture.hide()` line to hide the DOM element.

Now we're going to incorporate both the audio and video inputs into one sketch.  Write a sketch which draws a still frame everytime a clap is detected. 

Once you've got that working, instead of drawing still images on top of each other we're going to draw two more images to the right hand side of the first, so that you have three images horizonatlly across the page. Make sure you make the canvas bigger or you won't be able to see them.  

Write some code so that the first image is taken when the clap is detected, the second image half a second after the first and the third image half a second after that.  

You'll also need to make sure no new claps are detected while waiting for the second and third image to be captured.  For this we'll need to set up a timer so that you know how long has passed since the first clap.  You can use the [```millis()```](https://p5js.org/reference/#/p5/millis) function for this.  You'll also need to use a variable that keeps track of the current state of the program so that only one image is captured each time the state changes.

### Task 4 - Open task 
Taking input data from the camera and microphone do something cool. For example try using some part of the audio signal (e.g. amplitude, [frequency](https://p5js.org/examples/sound-frequency-spectrum.html)) to modify the camera input in some way.  Look at [this](https://p5js.org/examples/dom-video-pixels.html) example for some inspiration.
