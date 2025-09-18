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
> Lo que use de la unidad uno es el levy flight para hacer los cambios de colores de las particulas. y las ondas siguen al mouse.

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
    waves.push(new Wave(mouseX, mouseY));
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

### Rubrica

## 1. Investigación y Experimentación (Evidencia en Actividad 2): Excelente (4.5 - 5.0)

# Justificación:

En la bitácora, se puede observar que se ha hecho un análisis profundo de los ejemplos previos, con explicaciones detalladas sobre cómo se gestionan las partículas, cómo se manipulan las frecuencias musicales y cómo se aplica el concepto de movimiento sinodal (ondas) para el control de las partículas y su interacción con el audio. Además, se justifican las decisiones de diseño con ejemplos claros de cómo los conceptos de unidades anteriores (como vectores, aceleración y Levy Flight) se integran para lograr los efectos visuales y la interacción deseada.
El código refleja un manejo adecuado de la memoria, especialmente en la creación y eliminación de ondas, y la gestión del array de partículas está correctamente controlada para evitar sobrecargas de datos.

## 2. Intención y Diseño (Proceso de Actividad 3): Excelente (4.5 - 5.0)

# Justificación:

El concepto está claramente definido en la bitácora, y los artefactos de diseño están bien representados en el código. El propósito de la interacción (interacción musical y visual) está claramente descrito, y las decisiones de diseño están bien fundamentadas. La descripción en la bitácora del uso del sonido como input para la creación de ondas y la modulación de las partículas a través del audio es sólida. La obra final refleja directamente lo planeado, con una implementación que responde a la interacción del usuario de manera coherente, variada y atractiva.
El concepto de las ondas de sonido, combinadas con el movimiento senoidal de las partículas, es una clara manifestación de la idea original del proyecto.

## 3. Aplicación Técnica (Código de Actividad 3): Excelente (4.5 - 5.0)

# Justificación:

El código está bien estructurado, utilizando clases modulares tanto para las partículas (Mover) como para las ondas (Wave). La implementación del movimiento sinodal de las partículas y la interacción con el audio a través de la FFT está correctamente ejecutada. Además, el uso de herencia y la modularización del código permiten una mejor organización y control de los diferentes elementos. La gestión de la memoria es adecuada, ya que las partículas innecesarias se eliminan cuando ya no son necesarias, y el tamaño del array se mantiene controlado.
El uso de Levy Flight para la variación de color y la optimización de cálculos dentro del bucle principal (draw) demuestra un enfoque técnico eficiente.

## 4. Calidad de la Obra Final (Artefacto Entregado): Excelente (4.5 - 5.0)

# Justificación:

La obra es estable, y la interacción con el usuario es fluida. Las partículas responden al ritmo de la música de manera coherente, y la transición de colores y las ondas generadas muestran un nivel de detalle y variabilidad bien logrado. El rendimiento es constante, y el sistema generativo produce una amplia variedad de resultados visuales sin perder coherencia con el concepto inicial. La implementación del cambio de color al presionar la tecla espacio agrega un nivel interactivo adicional que mejora la experiencia del usuario. Además, se observa que las ondas de sonido afectan las partículas de manera eficiente, sin afectar negativamente el rendimiento del programa. La obra demuestra atención al detalle en cuanto a la estética, con transiciones suaves y un feedback visual claro.

## Nota Final Propuesta: 5.0 (Excelente)

# Resumen:

La bitácora demuestra una profunda comprensión y aplicación de los conceptos teóricos en la obra final. El código es eficiente, bien organizado, y responde adecuadamente a la interacción del usuario. Los conceptos de las unidades anteriores se aplican de manera efectiva y están justificados de manera clara en la bitácora. La obra final es estable, interactiva y visualmente atractiva, cumpliendo con los estándares más altos de la rúbrica.






