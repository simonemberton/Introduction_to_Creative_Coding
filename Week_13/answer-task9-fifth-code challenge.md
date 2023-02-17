# Week 13

## Answer - Task 9 fifth code challenge

Change the function setBg() so that it displays a gradient background.    

Task 9 should look like this:  

![alt text](./images/task9.jpg "drawing").


- Hopefully your sketch.js and html now looks something like this:  

- Your index.html code should like this

```html
  <!DOCTYPE html>
<html lang="">

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>p5.js example</title>
  <style>
    body {
      padding: 0;
      margin: 0;
    }
  </style>
  <script src="../p5.js"></script>
  <!-- <script src="../addons/p5.sound.js"></script> -->
</head>

<body>
  <!-- add a form element with text input and submit button -->
  <div>
    <form>
      <input type="text" id="myInput">
      <label for="bg">Choose a background:</label>
      <select id="bg">
        <option value="red">Red</option>
        <option value="green">Green</option>
      </select>
      <input type="submit" id="mySubmit" value="Submit">
    </form>
  </div>

  <!-- the P5 canvas is injected into main when you run the sketch -->
  <main>
  </main>

  <!-- add the javascript file at the bottom of the body tag -->
  <script src="sketch.js"></script>
</body>

</html>
```

- Your sketch.js code should like this

```javascript
let inputTxt = document.querySelector('#myInput');
let select = document.querySelector('#bg');
let submitBtn = document.querySelector('#mySubmit');

submitBtn.addEventListener('click', function(e) {
  e.preventDefault();
  console.log(inputTxt.value);
  console.log(select.value);
  setBg(select.value)
  greet(inputTxt.value);
});

function setup() {
  console.log("everything is working");
  createCanvas(710, 400);
  background(250);
}

function draw() {
  // put drawing code here
}

// sets the background gradient
function setBg(selectedColor) {
  console.log(selectedColor);
  let c1 = color(255);
  let c2 = color(selectedColor);

  // this uses 'lerp' to draw a horizontal line of a slightly different color for every pixel in the height of the sketch
  for(let y=0; y<height; y++){
    n = map(y,0,height,0,1);
    let newc = lerpColor(c1,c2,n);
    stroke(newc);
    line(0,y,width, y);
  }
}

// displays the text on the canvas
function greet(name) {
  for (let i = 0; i < 200; i++) {
    push();
    fill(random(255), 255, 255);
    translate(random(width), random(height));
    rotate(random(2 * PI));
    text(name, 0, 0);
    pop();
  }
}

```


