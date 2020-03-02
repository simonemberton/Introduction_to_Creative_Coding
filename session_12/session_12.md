# Session 12

## Parsing and displaying JSON data

### Task 1 - ISS

For our first task we're going to grab some data from an [API](http://api.open-notify.org) which provides real time data about the location (in latitude and longitude) of the International Space Station.  Paste the following into your browser to get that data in JSON format.

```javascript
http://api.open-notify.org/iss-now.json
```

You might also want to install a JSON formatter for your browser so that it's easier to read.  For example for Chrome you could use something like [this](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en). 

The following two p5 programs are examples which use the space station data and visualise it in ways that you've already seen when working with sensor data via an Arduino.  Yes that's right, you're already familiar with visualising data!  Get these sketches up and running and have a play with them.  Have a look at the data that is being logged to the console.  Also, notice how you use dot notation to access data when working with objects/JSON.

```javascript
// global variables
let iss_position;
let xPos = 0;

function setup(){
	createCanvas(windowWidth, windowHeight); // creates a canvas element
	background(8, 22, 64);
	frameRate(5); // reduces the frame rate to 5fps
	loadJSON("http://api.open-notify.org/iss-now.json", gotData); // loads the data from the API in JSON format
}

function gotData(data){
	// console.log(data); // print data to console
	iss_position = data.iss_position; // data into global variable
}

function draw(){

	if (iss_position) {
	loadJSON("http://api.open-notify.org/iss-now.json", gotData); // loads the data from the API in JSON format
	// console.log(iss_position.latitude); // print latitude value to console
	// console.log(iss_position.longitude); // print longitude value to console

	let mappedLat = map(iss_position.latitude, 90, -90, 0, height); // max and min latitude 90 -90 mapped to screen size
	stroke(0,200,255);
	point(xPos, mappedLat); // draw a point at this location

	let mappedLong = map(iss_position.longitude, -180, 180, 0, height); // max and min longitude -180 180 mapped to screen size
	stroke(200,255,0);
	point(xPos, mappedLong);
	
	// increment xPos
	if(xPos >= width) {
	   xPos = 0; 
	   background(8, 22, 64);   
	} else {
	   xPos++; 
	}

	}
}
```


```javascript
// global variables
let iss_position;
let circPlot = 0;
let cx, cy;

function setup(){
	createCanvas(windowWidth, windowHeight); // creates a canvas element
	background(8, 22, 64);
	frameRate(5); // reduces the frame rate to 5fps
	loadJSON("http://api.open-notify.org/iss-now.json", gotData); // loads the data from the API in JSON format
	cx = width/2;
		cy = height/2;
}

function gotData(data){
	// console.log(data); // print data to console
	iss_position = data.iss_position; // data into global variable
}

function draw(){
	fill(8, 22, 64, 3); // fill same colour as background with small alpha value
	rect(0,0,width,height); // draw rectangle same size as canvas

	if (iss_position) {
	loadJSON("http://api.open-notify.org/iss-now.json", gotData); // loads the data from the API in JSON format
	// console.log(iss_position.latitude); // print latitude value to console
	// console.log(iss_position.longitude); // print longitude value to console

	let mappedLat = map(iss_position.latitude, 90, -90, 0, height/2); // max and min latitude 90 -90 mapped to screen size
	let mappedLong = map(iss_position.longitude, -180, 180, 0, height/2); // max and min longitude -180 180 mapped to screen size

	// wrap around a circle
	if(circPlot > 360) {
		circPlot = 0;
	} else {
		circPlot++;
	}
	let angle = radians(circPlot);
	let x = cx + cos(angle) * mappedLat;
	let y = cy + sin(angle) * mappedLat;
	let x2 = cx + cos(angle) * mappedLong;
	let y2 = cy + sin(angle) * mappedLong;

	// draw shapes
	strokeWeight(0.5);
	stroke(0,200,255);
	noFill();
	ellipse(x,y,mappedLat, mappedLong);
	line(x,y,x2,y2);
	}
}
```



### Task 2 - OpenWeatherMap API

For this task we're going to use the OpenWeatherMap [API](https://openweathermap.org/api) to access current weather data.  If you follow the above link you'll see that under the heading `Current weather data` there is a link to the API documentation and a link to subscribe.  Firstly have a look at the API doc, here you’ll find information about how to access the data from the API.

For example to get the current weather in Bristol we use the following URL where `q` our query equals `Bristol`

```javascript
http://api.openweathermap.org/data/2.5/weather?q=Bristol
```

If you paste the above into a browser you'll notice that an access key is required.  Read about it [HERE](http://openweathermap.org/appid).

Let's go back to `Current weather data` and follow the link to `Subscribe`.  The free version is fine for our purposes. Click on `Get API key and Start`.

Once you’ve signed in you’ll be able to find your key under `API keys`.

Now we can try again to access the weather in Bristol
```javascript
http://api.openweathermap.org/data/2.5/weather?q=Bristol&appid=89cba66085cac69ac52e9d7c3adbd3b8
```

Now you’re able to access the current weather data let's get it into a p5 project and visualise it in some way.

### Task 3 - Visualising weather data

Here's some code to start you off:

```javascript
	let weather; // global variable

	function setup(){
		createCanvas(400, 400); // creates a canvas element
		loadJSON("http://api.openweathermap.org/data/2.5/weather?q=Bristol&units=metric&appid=89cba66085cac69ac52e9d7c3adbd3b8", gotData); // loads the data from the API in JSON format
	}

	function gotData(data){
		console.log(data); // print data to console
		weather = data; // assign data from API to weather variable
	}

	function draw(){
		background(0); // makes the background black
		if (weather){
			fill(255); // fill following shapes with white
			ellipse(width/2, height/2, weather.wind.speed*10); // draws a circle where the size is a scaled up from wind speed
		}
	}
```

It's making a call to the OpenWeatherMap API, don't forget to use your key and not mine!  We're also making use of a callback function ```gotData()``` and inside that function copying our data into the global variable ```weather```.

Have a look in the console to see the JSON data.  

In the ```draw()``` function of our p5 sketch we're taking the wind speed data and using it to scale a cirle, so far not a very interesting visualisation! 

### Task 4 - Weather vane

Now let's try and do something more interesting with the incoming data.  Try drawing a weather vane which points in the direction of the wind and is scaled depending on the wind speed.

The following p5 functions may come in handy:

* [```translate()```](https://p5js.org/reference/#/p5/translate) to change the point of origin (e.g. point around which the shape rotates).

* [```angleMode(DEGREES)```](https://p5js.org/reference/#/p5/angleMode§) as p5 uses radians as default.

* [```rotate()```](https://p5js.org/reference/#/p5/rotate) to rotate.

### Task 5 - Data on demand

Now you've got that working the next task is to add some interactivity.  First we are going to add a text box so that a user can enter the name of the city that they want to get weather data about.  We'll go to our ```index.html``` file and add the following code to the body of our webpage:

```javascript
<p>
	City: <input id = "city" value="Bristol"> </input>
	<br/>
	<button id = "submit">submit</button>
</p>
```

Nice! Now a user can write some text input into the box.  We'll need to splice this text input into our call to the API.  

Going back to our ```sketch.js``` file let's create three variables for the various components that make up our call to the API.  The first contains the URL information before the city query (http://api.openweathermap.org/data/2.5/weather?q=), the second is the city query (Bristol) and the third the URL information after the city query (&units=metric&appid=89cba66085cac69ac52e9d7c3adbd3b8).  Now create another variable with uses the addition (+) operator to concatenate the variables contained in the separate parts of the URL together.

We're going to call a function (```callAPI()```) everytime the submit button is pressed.  We'll use the id that we assigned to our text box (```#submit```) so that we know when it's been pressed, we'll also create a new variable that holds the city name input to the text box (```#city```).  Write the following code in the ```setup()``` function:

```javascript
	let button = select('#submit');
	button.mousepressed(callAPI);

	input = select(#city); 
```

Now let's create a new function called ```callAPI``` which includes all the elements that make up the API call.  We can use ```input.value()``` to get the city that is input by the user.  In this function we'll also need the ```loadJSON()``` function to make the call to the API.

```javascript
function callAPI(){
	let newUrl = urlBeforeCity + input.value() + urlAfterCity;
        loadJSON(newUrl, gotData);
}
```

If you've managed to get all that working you should now be able to change the direction and scale of your weather vane depending on the city you search for.

### Task 6 - Blowing in the wind

For this task you need to add some code so that your weather vane moves around the screen in the direction the wind is blowing. 

If you need some help have a look at this p5 [example](https://p5js.org/examples/hello-p5-weather.html).  See how they have an ellipse moving around the screen in the direction of the wind.  

This example also has a weather vane in the corner but I'm sure it's not as good as yours!

### Extra challenge

* Add code from recent weeks so that your weather vane has sonified particles that are attracted to it.
