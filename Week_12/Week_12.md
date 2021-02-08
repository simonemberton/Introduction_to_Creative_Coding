# Week 12

## HTML, Pure JavaScript and Interaction with the DOM

### Task 1 - HTML Page

In line with doing some independent learning, some of the following instructions are structured to get you to do your own research to find the answers.

Create a new folder on your machine called DOM_DEMO_01 or something that is in line with your previous naming system.

Create a new index.html with all the necessary mark up that is required:

Look in the slides to find the example of the basic page - *DO NOT COPY AND PASTE*

Make sure you save it as "index.html"


In the body of your index.html create a pair of ```<script>``` tags

Inside the tags, add the line:
```javascript 
document.write("Hello Whirled");
```

Open your page with the browser, congratulations, you’ve written your first piece of pure DOM JS code!


### Task 2 - Button Alert

We're going to create a pop up alert in our browser.

In the head of your page add a pair of script tags.

Create a function called sayHello in the script tags.

In the function, define an alert method with a string as the argument - it could be any string.

In the body, create a button with the text "Don't Press!" inside, which calls the sayHello method from its onclick attribute. https://www.w3schools.com/jsref/event_onclick.asp

Run your page and you have done your first piece of interactive pure JS coding. 

Try saving the script in the head as a separate .js file called demo.js and use the <script> tag's src attribute to embed it in the html page.


### Task 3 - Animating a div

We're going to spin a HTML element round... you know, for laughs and that? And perhaps a bit of learning too:

Create a div in the body of your html file with an id of “divAnim01”: https://www.w3schools.com/tags/tag_div.asp

Insert some text and an image inside the div tags. https://www.w3schools.com/tags/tag_img.asp. It's up to you to choose an image and download it. We chose the p5 logo from [here](https://medium.com/processing-foundation/making-p5-js-accessible-e2ce366e05a0).

Add two buttons, one for start and one for stop.

For the onclick attribute add the ```start()``` and ```stop()``` values for the corresponding buttons, these will link to functions in your JavaScript file.

In your new demo.js file that is linked in your html, declare a ```let``` called deg and initialise it to 0.

Declare another ```let``` called rotationDiff and initialise it to 1.

```javascript
let deg = 0; // starting point
let rotationDiff = 1;

let rotation;
let rotationInterval;

```

Declare two further variables called rotation and rotationInterval and leave them uninitialised. 

Define three functions, one called ```start()```, one called ```stop()``` and one called ```rotateDiv()```.

Inside the ```start()``` function, assign the variable rotationInterval to the method ```setInterval("rotateDiv()", 10);``` 
And also write the statement ```console.log(“start”);``` This will call the ```rotateDiv()``` function every 10 milliseconds.

Inside the ```stop()``` function, write the statement ```clearInterval(rotationInterval);``` And also write the statement ```console.log(“stop”);``` This will clear the calling of the ```rotateDiv()``` function.

```javascript
function start() {
    rotationInterval = setInterval("rotateDiv()", 10);
    console.log("start");
}

function stop() {
  clearInterval(rotationInterval);
  console.log("stop");
}
```

In the ```rotateDiv()``` function, declare a ```let``` called divAnim01 and assign it to the id in your html document by using the method ```document.getElementById(“divAnim01");```

Add the statement ```divAnim01.style.transform = "rotate(" + deg + “deg)";```. This will work for chrome, and will perform the actual rotation using the CSS3 rotate function. In the code below, all the other transform functions are also defined for other browsers.

```javascript
function rotateDiv() {
    let divAnim01 = document.getElementById("divAnim01");

    //divAnim01.style.webkitTransform = "rotate(" + deg + "deg)";
    divAnim01.style.transform = "rotate(" + deg + "deg)";
    // divAnim01.style.MozTransform = "rotate(" + deg + "deg)";
    // divAnim01.style.msTransform = "rotate(" + deg + "deg)";
    // divAnim01.style.OTransform = "rotate(" + deg + "deg)";
    deg += rotationDiff;
    if(deg > 360) {
     deg = 0;
    }
    
}
```
Here's what you might be looking at now:

![alt text](spinningdiv.gif "SpinningDiv")

### Task 4 - Independent Learning Challenge: Explore the way that p5 interacts with the DOM

Can you get some examples working on your own? In a blank sketch, try creating a slider, then try creating a button.

The link is [here](https://p5js.org/reference/), scroll down to the section called DOM and check out some of the examples.

### Task 5 Stretch Task: Multiplication Calculator 

Create a web form that takes in two numbers and multiplies them together when the form is submitted.

It should look something like this:

![alt text](calculator.png "Calculator")

Here are some resources for you to follow:

[Little tutorial](https://www.w3resource.com/javascript-exercises/javascript-basic-exercise-10.php)

[Maths in Js](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Math)

There are many different ways of doing this. One solution will be posted on Blackboard at the end of the session.

<!--### Task 6 - DOM Tutorial Online-->

<!--http://www.w3schools.com/js/js_htmldom.asp-->



<!--### Task 7 - Advanced: Sidenav tutorial-->

<!--https://www.w3schools.com/howto/howto_js_sidenav.asp-->















