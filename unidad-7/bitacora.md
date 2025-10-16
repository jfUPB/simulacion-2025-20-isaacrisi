# Evidencias de la unidad 7



## Actividad 1

- Los que mas me llamaron la atencion fueron el de vertigo porque da esa sensacion de lo que es el vertigo, tambien me gusto mucho el de clock porque es muy facil de leer y de enetnder a pesar que las letras "no estan en orden" lo otro es la de homosexual y heterosexual que me saco una risa porque logra hacer humos respecto al tema sin discriminar y se entiende perfectamente.

- Las palabras que se me pueden ocurrir asi simples seria punch y que la p funcione como el puño y le pegue a las demas letras, la otra podria ser demon poniendole como cuernos en la d y la n y una sonrisa abajo de la palabra para que la e y la o actuen como ojos y la m una nariz.

## Actividad 2

# Bitácora – Experimentos con Matter.js y p5.js

## 1. Exploración inicial
Se observó el video de Patt Vira sobre Matter.js y se revisaron los ejemplos básicos del sitio oficial, entendiendo cómo crear mundos físicos, añadir cuerpos y manipularlos con el mouse.

## 2. Conceptos clave

| Concepto | Explicación breve |
|-----------|-------------------|
| **Engine** | Motor que calcula fuerzas, colisiones y movimientos. |
| **World** | Escenario donde se agregan los cuerpos físicos. |
| **Bodies** | Objetos que forman el mundo (rectángulos, círculos, polígonos). |
| **Constraint** | Restricción que une cuerpos o los fija a un punto. |
| **MouseConstraint** | Permite arrastrar cuerpos con el mouse. |
| **Runner / Events** | Controla la simulación y permite manejar eventos físicos. |

## 3. Experimentos básicos

### Experimento 1: Mundo con gravedad y cuerpos simples
Crea un mundo con gravedad, cuerpos dinámicos y un suelo estático.

```javascript
  // Experimento 1 - Mundo con gravedad y cuerpos simples
  const Engine = Matter.Engine;
  const World = Matter.World;
  const Bodies = Matter.Bodies;
  
  let engine, world;
  let boxes = [];
  let ground;
  
  function setup() {
    createCanvas(600, 400);
    engine = Engine.create();
    world = engine.world;
  
    let options = { isStatic: true };
    ground = Bodies.rectangle(300, height - 10, 600, 20, options);
    World.add(world, ground);
  }
  
  function mousePressed() {
    let box = Bodies.rectangle(mouseX, mouseY, random(20, 50), random(20, 50));
    let circle = Bodies.circle(mouseX, mouseY, random(10, 25));
    World.add(world, [box, circle]);
    boxes.push(box, circle);
  }
  
  function draw() {
    background(200);
    Engine.update(engine);
  
    fill(127);
    for (let b of boxes) {
      if (b.circleRadius) {
        ellipse(b.position.x, b.position.y, b.circleRadius * 2);
      } else {
        rectMode(CENTER);
        rect(b.position.x, b.position.y, 50, 50);
      }
    }
  
    fill(100);
    rectMode(CENTER);
    rect(ground.position.x, ground.position.y, 600, 20);
  }
```
<img width="604" height="400" alt="image" src="https://github.com/user-attachments/assets/5d9c5ef3-11e6-4f10-98b8-4f8ab22bebd7" />

```javascript
// Experimento 2 - Interacción con MouseConstraint
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;
const Mouse = Matter.Mouse;
const MouseConstraint = Matter.MouseConstraint;

let engine, world;
let ground;
let boxes = [];
let mConstraint;

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  world = engine.world;

  let options = { isStatic: true };
  ground = Bodies.rectangle(300, height - 10, 600, 20, options);
  World.add(world, ground);

  for (let i = 0; i < 5; i++) {
    let b = Bodies.rectangle(100 + i * 100, 50, 40, 40);
    boxes.push(b);
    World.add(world, b);
  }

  let canvasMouse = Mouse.create(canvas.elt);
  let optionsMC = { mouse: canvasMouse };
  mConstraint = MouseConstraint.create(engine, optionsMC);
  World.add(world, mConstraint);
}

function draw() {
  background(220);
  Engine.update(engine);

  for (let b of boxes) {
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    rectMode(CENTER);
    fill(150, 100, 200);
    rect(0, 0, 40, 40);
    pop();
  }

  fill(100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 20);
}
```

<img width="615" height="406" alt="image" src="https://github.com/user-attachments/assets/3436aabc-1d50-4d79-9dc3-4072ceb3cf77" />


la mayor dificultad fue poder configurar la libreria






