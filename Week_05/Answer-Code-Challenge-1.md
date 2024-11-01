# Week 05

## Task 2 First Code Challenge Answer

- Hopefully your sketch and console output now looks something like this:  

<p align="center">
  <img src="./images/task2-code-challenge.png">
</p>

- Your code should like this

```javascript
  // Global variables
  let myWords = ["Every", "girl", "deserves", "to", "take", "part", "in", "creating", "the", "technology", "that", "will", "change", "our world"];
  let xVal = 20;

  function setup() {
    createCanvas(1024, 500);
    background(color(200));
    noLoop();
  }

  function draw() {
    for (var i = 0; i < myWords.length; i++) {
      console.log(myWords[i]);
      console.log(xVal);
      textAlign(CENTER);
      text(myWords[i], xVal, 100);
      xVal += 50;
    }
  }
```
