# Session 12

## Parsing and displaying JSON data

### Task 1 - OpenWeatherMap API

For this task we're going to use the OpenWeatherMap [API](https://openweathermap.org/api) to access current weather data.  If you follow the above link you'll see that under the heading `Current weather data` there is a link to the API documentation and a link to subscribe.  Firstly have a look at the API doc, here you’ll find information about how to access the data from the API.

For example to get the current weather in Bristol we use the following URL where `q` our query equals `Bristol`

```javascript
api.openweathermap.org/data/2.5/weather?q=Bristol
```

If you paste the above into a browser you'll notice that an access key is required.  Read about it [HERE](http://openweathermap.org/appid).

Let's go back to `Current weather data` and follow the link to `Subscribe`.  The free version is fine for our purposes. Click on `Get API key and Start`.

Once you’ve signed in you’ll be able to find your key under `API keys`.

Now we can try again to access the weather in Bristol
```javascript
api.openweathermap.org/data/2.5/weather?q=Bristol&appid=89cba66085cac69ac52e9d7c3adbd3b8
```

Now you’re able to access the current weather data let's get it into a p5 project and visualise it in some way.

### Task 2 - Visualising weather data

Here's some code to start you off:

```javascript
	var weather; // global variable

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

### Task 3 - Weather vane

Now let's try and do something more interesting with the incoming data.  Try drawing a weather vane which points in the direction of the wind and is scaled depending on the wind speed.

The following p5 functions may come in handy:

* [```translate()```](https://p5js.org/reference/#/p5/translate) to change the point of origin (e.g. point around which the shape rotates).

* [```angleMode(DEGREES)```](https://p5js.org/reference/#/p5/angleMode§) as p5 uses radians as default.

* [```rotate()```](https://p5js.org/reference/#/p5/rotate) to rotate.

### Task 3 - Data on demand

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
	var button = select('#submit');
	button.mousepressed(callAPI);

	input = select(#city); 
```

Now let's create a new function called ```callAPI``` which includes all the elements that make up the API call.  We can use ```input.value()``` to get the city that is input by the user.  In this function we'll also need the ```loadJSON()``` function to make the call to the API.

```javascript
    function callAPI(){
        var newUrl = urlBeforeCity + input.value() + urlAfterCity;
        loadJSON(newUrl, gotData);
    }
```

If you've managed to get all that working you should now be able to change the direction and scale of your weather vane depending on the city you search for.

### Task 4 - Blowing in the wind

For this task you need to add some code so that your weather vane moves around the screen in the direction the wind is blowing. 

If you need some help have a look at this p5 [example](https://p5js.org/examples/hello-p5-weather.html).  See how they have an ellipse moving around the screen in the direction of the wind.  

This example also has a weather vane in the corner but I'm sure it's not as good as yours!

### Extra challenge

* Add code from recent weeks so that your weather vane has sonified particles that are attracted to it.

