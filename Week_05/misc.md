# Week 05


- My code looks like this

```javascript
  let animals = ["cat", "dog", "horse"];

  for (let i = 0; i < animals.length; i++) {
    console.log(animals[i]);
  }
  
  animals.pop();
  // ["cat", "dog"]


  animals.push("giraffe");
  // ["cat", "dog", "horse", "giraffe"]

  animals[1]
  // dog

  animals[2]
  // horse

  console.log(animals);


  function setup() {
    let j = 3;
  }

  function draw() {
    console.log(j); // >> Uncaught ReferenceError: j is not defined
    console.log(i); // >> 5
  }

 
```
