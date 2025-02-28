# Week 16

## Answer - Task 2 - Pipes - Second and Third code challenge
### Randomnly generate the heights of the bars and make them move across the screen


- Your pipe.js code should like this to move the pipe and randomnly generate their heights.  
- Im my example code the gap between them stays the same.  

```javascript
class Pipe
{
  constructor()
  {
    this.x = width-50; // start nearly off screem
    this.origin = random(height-100);
    this.gap = 200;
  }
  
  show()
  {
    fill(220);
    // rect(x, y, w, h);
    // the gap rectangle
    rect(this.x, this.origin - (this.gap/2), 55, this.gap); // comment out to see the gap!
    fill(120);
    // top bar
    rect(this.x, 0, 55, this.origin - (this.gap/2));
    // bottom
    rect(this.x, this.origin + (this.gap/2), 55, height);
  }

  update()
  {
    // move across screen
    this.x --;
  }
}
```


![alt text](./images/bars-moving.gif "bars moving")  