# Week 05

## Task 1 - Answer

- Hopefully your sketch and console output now looks something like this:  

<p align="center">
  <img src="./images/task1.png">
</p>

- Your code should like this

```javascript
  // Global variables
  let myWords = ["Every", "girl", "deserves", "to", "take", "part", "in", "creating", "the", "technology", "that", "will", "change", "our world"];

  function setup() {
    console.log('hello world');
    createCanvas(1024, 500);
    background(color(200));
    noLoop();
  }

  function draw() {
    // this for loop outputs each element from the Array myWords
    for (var i = 0; i < myWords.length; i++) {
      console.log(myWords[i]);
    }
  }
```
