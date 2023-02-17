# Week 13

## Answer - Task 5 second code challenge

Edit the code in index.html and sketch.js add an HTML form 'select' element to your html page.      

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
});
```

![alt text](./images/task5.jpg "console"). 