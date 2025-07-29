# Unidad 2

## 🔎 Fase: Set + Seek


### Actividad 1

- ¿Cómo funciona la suma dos vectores en p5.js?

  Se suma componente a componente
  
- ¿Por qué esta línea position = position + velocity; no funciona?

  Porque en esta biblioteca de javascript no hay sobrcarga para la suma por lo que no puede sumar vectores.

### Actividad 2

-
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
  this.position = createVector(width, height);
  this.speed = createVector(2,2.5);
  
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.position.x,this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x+this.speed.x;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y+this.speed.y;
    } else {
      this.position.y--;
    }
  }
}
```
