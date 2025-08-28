# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> aca use el concepto de las ondas, por un lado dandole un movimiento sinoidal a las particulas, por e l otro usando sonido como tal y sus frecuencias para hacer cosas en el codigo.

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Lo que apliqué de la unidad 3 es el cambio de la aceleracion, en este caso va con el ritmo de la musica.

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> El concepto de la unidad 2 que usé es el motion 101 y el uso de vectores para el movimiento.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Lo que use de la unidad uno es el levy flight para hacer los cambios de colores de las particulas.

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
> Al presionar la tecla espacio se puede cambiar el color de las ondas del fondo. 

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/isaacrisi/sketches/cLQI1AlXS)

## Código de la obra 

``` js
let song;
let fft;
let movers = [];
let numMovers = 50;

let waves = []; // <- array de ondas
let beatThreshold = 100; // <- ajustar según la canción
let lastBeatTime = 0;
let beatDelay = 300; // milisegundos entre beats detectables
let currentWaveColor;


function preload() {
  song = loadSound('enemy.mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  fft = new p5.FFT();
  song.play();
  
  currentWaveColor = color(random(255), random(255), random(255)); // color inicial

  for (let i = 0; i < numMovers; i++) {
    movers.push(new Mover());
  }
}

function keyPressed() {
  if (key === ' ') {
    currentWaveColor = color(random(255), random(255), random(255));
  }
}


function draw() {
  background(0, 20);

  let spectrum = fft.analyze();
  let energy = fft.getEnergy("highMid");

  // 🧠 Detectar beat
  if (energy > beatThreshold && millis() - lastBeatTime > beatDelay) {
    waves.push(new Wave(width / 2, height / 2)); // <- onda nueva desde el centro
    lastBeatTime = millis();
  }

  // 💫 Dibujar ondas
  for (let i = waves.length - 1; i >= 0; i--) {
    waves[i].update();
    waves[i].display();
    if (waves[i].isFinished()) {
      waves.splice(i, 1);
    }
  }
  console.log(energy);

  // 🎯 Mover partículas
  for (let mover of movers) {
    mover.update(energy);
    mover.display();
  }
}

// 🎵 Partícula
class Mover {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D();
    this.acc = createVector();
    this.color = createVector(random(255), random(255), random(255));
    this.size = random(8, 16);
    this.phase = random(TWO_PI); // fase aleatoria para movimiento único

  }

  update(energy) {
  let t = millis() / 1000; // tiempo en segundos
  let freq = map(energy, 0, 255, 0.5, 2); // frecuencia basada en la energía
  let amp = map(energy, 0, 255, 10, 100); // amplitud basada en la energía

  // Movimiento senoidal en X e Y
  this.pos.x += sin(t * freq + this.phase) * 4;
  this.pos.y += cos(t * freq + this.phase) * 4;

  // Aseguramos que se queden dentro de la pantalla
  if (this.pos.x < 0 || this.pos.x > width) this.phase += PI; // invertir fase en los bordes
  if (this.pos.y < 0 || this.pos.y > height) this.phase += PI;

  // Cambiar color ligeramente con ruido
  let step = levyFlight();
  this.color.x = constrain(this.color.x + step.x * 0.5, 0, 255);
  this.color.y = constrain(this.color.y + step.y * 0.5, 0, 255);
  this.color.z = constrain(this.color.z + step.z * 0.5, 0, 255);

}


  display() {
    noStroke();
    fill(this.color.x, this.color.y, this.color.z, 180);
    ellipse(this.pos.x, this.pos.y, this.size, this.size);
  }
}

// 🌊 Clase para ondas
class Wave {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.radius = 10;
    this.alpha = 255;
    this.color = currentWaveColor; // usar color actual al momento de crear
  }

  update() {
    this.radius += 5;      // velocidad de expansión
    this.alpha -= 5;       // desvanecimiento
  }

  display() {
    noFill();
    stroke(this.color.levels[0], this.color.levels[1], this.color.levels[2], this.alpha);
    strokeWeight(2);
    ellipse(this.pos.x, this.pos.y, this.radius * 2);
  }

  isFinished() {
    return this.alpha <= 0;
  }
}

// 🌀 Función Levy Flight para color
function levyFlight() {
  function randomLevy() {
    let r = randomGaussian() * 25;
    return constrain(r, -100, 100);
  }
  return createVector(randomLevy(), randomLevy(), randomLevy());
}

```

## Captura de pantalla representativa


<img width="876" height="686" alt="image" src="https://github.com/user-attachments/assets/84b65fa6-6393-4af1-ba36-66b6ad14aa81" />







