# Week 13

## Answer - Task 3 First Code Challenge

Edit the code in sketch.js so that your event listener 'click' triggers the message "This message has been added by JavaScript" to appear on the page.      

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
  <div id="myId"></div>
  <a class="myClass" href="#">Click me</a>
   <script src="sketch.js"></script>
</body>

</html>
```

- Your sketch.js code should like this

```javascript
  console.log("working");

function injectIntoPage() {
  let myDiv = document.querySelector("#myId");
  console.log(myDiv);
  myDiv.innerHTML = "This message has been added by JavaScript";
}

let myLink = document.querySelector('.myClass');
myLink.addEventListener('click', injectIntoPage);
```
