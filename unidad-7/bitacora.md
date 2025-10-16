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


## Apply

# Animación Física: “PUNCH”

## Palabra elegida
**PUNCH**

## Idea conceptual
La animación representa la palabra **“PUNCH”** (“golpe”) haciendo que la **letra P actúe como un puño** que se lanza con fuerza hacia las demás letras, empujándolas y provocando colisiones y rebotes. Este movimiento comunica visualmente el impacto y energía del concepto.

## Aspectos técnicos
- Cada letra se modeló como un **cuerpo rectangular** de Matter.js, usando `Bodies.rectangle()`.
- La **“P”** recibe una **velocidad inicial alta** (`Body.setVelocity`) y se ubica más a la izquierda para simular el recorrido del golpe.
- Las demás letras reaccionan mediante el motor físico con **colisiones elásticas** (restitución = 0.6).
- El mundo tiene **gravedad leve** y **límites invisibles** (suelo y paredes).
- No se usaron restricciones; toda la interacción es libre y dinámica.
- Al hacer clic se **reinicia la animación**, permitiendo repetir el golpe.

## Código completo

```javascript
// Animación: "PUNCH" donde la P golpea a las demás letras con más fuerza

// Módulos Matter.js
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;
const Body = Matter.Body;

let engine, world;
let letters = [];
let ground, leftWall, rightWall;

function setup() {
  createCanvas(800, 400);
  engine = Engine.create();
  world = engine.world;

  // Gravedad leve
  engine.world.gravity.y = 0.5;

  // Suelo y paredes invisibles
  let options = { isStatic: true };
  ground = Bodies.rectangle(400, height, 800, 50, options);
  leftWall = Bodies.rectangle(0, 200, 50, 400, options);
  rightWall = Bodies.rectangle(800, 200, 50, 400, options);
  World.add(world, [ground, leftWall, rightWall]);

  // Crear letras como cuerpos rectangulares
  const word = "PUNCH";
  let startX = 250;

  for (let i = 0; i < word.length; i++) {
    let letter = word[i];
    let box = Bodies.rectangle(startX + i * 80, 200, 60, 80, {
      restitution: 0.6,
      friction: 0.2,
      density: 0.002
    });
    box.label = letter;
    letters.push(box);
  }

  World.add(world, letters);

  // Mover la "P" más lejos a la izquierda y darle más velocidad
  Body.setPosition(letters[0], { x: 80, y: 200 });
  Body.setVelocity(letters[0], { x: 15, y: -3 });
}

function draw() {
  background(230);
  Engine.update(engine);

  // Dibujar letras
  textAlign(CENTER, CENTER);
  textSize(64);
  noStroke();
  fill(30);

  for (let b of letters) {
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    text(b.label, 0, 10);
    pop();
  }
}

// Reiniciar animación con clic
function mousePressed() {
  const word = "PUNCH";
  let startX = 250;
  for (let i = 0; i < letters.length; i++) {
    Body.setPosition(letters[i], { x: startX + i * 80, y: 200 });
    Body.setVelocity(letters[i], { x: 0, y: 0 });
    Body.setAngle(letters[i], 0);
  }
  // Recolocar la "P" más a la izquierda y con más fuerza
  Body.setPosition(letters[0], { x: 80, y: 200 });
  Body.setVelocity(letters[0], { x: 15, y: -3 });
}
```
https://editor.p5js.org/isaacrisi/sketches/exnEg-9mZ

<img width="795" height="376" alt="image" src="https://github.com/user-attachments/assets/c54c957c-92bd-4d55-997a-75225c0313a7" />



![Grabación 2025-10-16 132542](https://github.com/user-attachments/assets/2f62a815-eb07-48e6-a6a1-2e21002228c2)








