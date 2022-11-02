# Week 05

## Task 2 Second Code Challenge Answer

- Hopefully your sketch looks something like this:  

<p align="center">
  <img src="./images/task3-code-challenge.png">
</p>

- Your code should like this

```javascript
  // Global variables
  let myWords = ["Every", "girl", "deserves", "to", "take", "part", "in", "creating", "the", "technology", "that", "will", "change", "our world"];
  let xVal = 30;
  let yVal = 30;

  //Every girl deserves to take part in creating the technology that will change our world, and change who runs it.
  //Malala Yousafzai, Nobel Peace Prize Winner

  function setup() {
    console.log('hello world');
    createCanvas(1024, 500);
    background(color(200));
    noLoop();
    myWords.push("and");
    myWords.push("change");
    myWords.push("who");
    myWords.push("runs");
    myWords.push("it.");
    console.log(myWords);
  }

  function draw() {
    for (var i = 0; i < myWords.length; i++) {
      console.log(myWords[i]);
      console.log(xVal);
      fill(200, 102, 153);
      textSize(12);
      textAlign(CENTER);
      //text(myWords[i], random(width), random(height));
      text(myWords[i], xVal, yVal);
      xVal += 43;
      yVal += 20;
    }
  }
```
