# Session 09

## Interaction beyond keyboard and mouse 


### Task 1 - Create audio-responsive visualisation

Have a look at the [`p5.AudioIn`](https://p5js.org/reference/#/p5.AudioIn) example and get it working on your machine.

In this example the `getLevel()` function is being used to access the volume or amplitude coming into the microphone.  Print these values to the console while making some noises.  Can you see how they change?  

Now draw a shape to the screen and change the size of the shape depending on the level of amplitude coming in from the microphone.

### Task 2 - Clap detection

Now set a threshold value which can be used to detect a clapping sound.  If the amplitude is over this threshold do something e.g. change the background colour.

### Task 3 - Camera input
Now we are going to input some live data from a camera.  Check out the [Video Capture ](https://p5js.org/examples/dom-video-capture.html) example.

Notice you have two video images on the screen.  One is the DOM element and one is the image that you're drawing in the `draw()` function.  Try commenting out the `capture.hide()` line to hide the DOM element.

Now we're going to incorporate both the audio and video inputs into one sketch.  Write a sketch which draws a still frame everytime a clap is detected. 

Once you've got that working, instead of drawing still images on top of each other we're going to fill the whole page with a history of still images. First create a new global array variable.  Then each time a clap is detected push the current video frame into the array using `video.get()`.  Now in the draw function loop through the array and draw each image that is stored in the array in a grid pattern across the page ([Muybridge-style](https://en.wikipedia.org/wiki/Eadweard_Muybridge#/media/File:The_Horse_in_Motion_high_res.jpg)).

### Task 4 - Open task 
Taking input data from the camera and microphone do something cool. For example try using some part of the audio signal (e.g. amplitude, [frequency](https://p5js.org/examples/sound-frequency-spectrum.html)) to modify the camera input in some way.
