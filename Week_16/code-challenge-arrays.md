# Week 16

### Code challenge (in the recap slides): Talk is cheap show me the code

In your function create a loop to print out the every item in the array to the console if it contains the letter ‘i’.  Amend it to print out every item that contains two vowels…


<p align="center">
<img src="./assets/analogue-clock.gif" alt="Drag" width="50%"/>
</p>

Your ```sketch.js``` should look as follows:  

```javascript
let words = ["talk", "is", "cheap", "show", "me", "the", "code"];

function setup() {
  createCanvas(400, 400);
  let result = findWordsWithTwoVowels(words);
  console.log("Words with at least two vowels:", result);
}

function findWordsWithTwoVowels(wordArray) {
  let vowels = ['a', 'e', 'i', 'o', 'u'];
  let wordsWithTwoVowels = [];

  for (var i = 0; i < wordArray.length; i++) {
    let vowelCount = 0;
    let word = wordArray[i];
    for (let j = 0; j < word.length; j++) {
      if (vowels.includes(word[j].toLowerCase())) {
        vowelCount++;
      }
    }
    if (vowelCount >= 2) {
      wordsWithTwoVowels.push(word);
    }
  }

  return wordsWithTwoVowels;
}
```