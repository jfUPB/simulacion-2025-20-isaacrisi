# Unidad 3

## 🔎 Fase: Set + Seek

### Codigo de Fluidos

https://editor.p5js.org/isaacrisi/sketches/Y3ppeDCGz

Modelé la fuerza de movimiento usando Perlin noise para crear trayectorias suaves y orgánicas, y la resistencia con una fuerza de arrastre proporcional al cuadrado de la velocidad cuando el objeto entra en un área líquida. Estas fuerzas simulan comportamientos naturales y se conectan conceptualmente con la obra generativa al crear trayectorias impredecibles pero armoniosas, como sistemas naturales que responden a entornos invisibles. La estética se refuerza con formas de estrella y trazos translúcidos que dejan estelas vivas y cambiantes.

<img width="872" height="439" alt="image" src="https://github.com/user-attachments/assets/5d544a3e-713a-4199-9e83-39130af00ca4" />

``` js
let movers = [];
let liquid;

function setup() {
  createCanvas(800, 400);
  background(0);
  liquid = new Liquid(width / 3, height / 3, width / 3, height / 3, 0.2);
  reset();
}

function draw() {
  // Fondo con opacidad para estelas
  fill(0, 10);
  noStroke();
  rect(0, 0, width, height);

  // Dibujar el charco
  liquid.show();

  for (let i = 0; i < movers.length; i++) {
    let mover = movers[i];

    // Movimiento generado por ruido Perlin
    let angle = noise(mover.noiseOffset) * TWO_PI * 2;
    let force = p5.Vector.fromAngle(angle);
    force.setMag(0.5); // Fuerza constante mínima
    mover.noiseOffset += 0.01; // Avanzar el ruido

    // Aplicar fuerza de movimiento perlin
    mover.applyForce(force);

    // Si está dentro del líquido, aplicar resistencia
    if (liquid.contains(mover)) {
      let drag = liquid.calculateDrag(mover);
      mover.applyForce(drag);
    }

    mover.update();
    mover.show();
    mover.checkEdges();
  }
}

function reset() {
  for (let i = 0; i < 30; i++) {
    let x = random(width);
    let y = random(height);
    let mass = random(1, 4);
    let mover = new Mover(x, y, mass);
    mover.noiseOffset = random(1000); // offset único por mover
    movers[i] = mover;
  }
}

// ---------------------
// LIQUID
// ---------------------
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  contains(mover) {
    let pos = mover.position;
    return (
      pos.x > this.x &&
      pos.x < this.x + this.w &&
      pos.y > this.y &&
      pos.y < this.y + this.h
    );
  }

  calculateDrag(mover) {
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;
    let drag = mover.velocity.copy();
    drag.mult(-1);
    drag.setMag(dragMagnitude);
    return drag;
  }

  show() {
    noStroke();
    fill(163, 200, 255, 255); // Azul translúcido
    rect(this.x, this.y, this.w, this.h);
  }
}

// ---------------------
// MOVER
// ---------------------
class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 6;
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 1));
    this.acceleration = createVector(0, 0);
    this.color = color(random(100, 255), random(100, 255), random(100, 255), 180);
    this.noiseOffset = random(1000);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

    show() {
    noStroke();
    fill(this.color);
    drawStar(this.position.x, this.position.y, this.radius, this.radius / 2, 5);
  }


  checkEdges() {
    if (this.position.x > width + this.radius) {
      this.position.x = -this.radius;
    } else if (this.position.x < -this.radius) {
      this.position.x = width + this.radius;
    }

    if (this.position.y > height + this.radius) {
      this.position.y = -this.radius;
    } else if (this.position.y < -this.radius) {
      this.position.y = height + this.radius;
    }
  }
}

function drawStar(x, y, radius1, radius2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius2;
    let sy = y + sin(a) * radius2;
    vertex(sx, sy);
    sx = x + cos(a + halfAngle) * radius1;
    sy = y + sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}
function keyPressed() {
  if (key === ' ') {
    // Tamaño mínimo y máximo del charco
    let newW = random(50, width / 1.5);
    let newH = random(50, height / 1.5);

    // También podemos cambiar su posición para que no se salga del canvas
    let newX = random(0, width - newW);
    let newY = random(0, height - newH);

    // Asignar nuevos valores
    liquid.x = newX;
    liquid.y = newY;
    liquid.w = newW;
    liquid.h = newH;
  }
}

```

### Codigo de Friccion 

https://editor.p5js.org/isaacrisi/sketches/fIbbwBZiA

Modelé la fuerza de gravedad como un vector constante proporcional a la masa que simula el peso, la fricción aparece solo cuando los objetos tocan el suelo y actúa en dirección opuesta a la velocidad, y el viento se aplica como una ráfaga aleatoria al hacer clic generando variaciones impredecibles. Estas fuerzas reflejan comportamientos físicos simples y al usarlas dentro de una obra generativa, producen movimientos únicos y estéticas emergentes con estelas que visualizan el paso del tiempo, el caos y la interacción con el entorno. Cada esfera deja un rastro colorido que enfatiza el flujo y la transformación continua.

<img width="706" height="527" alt="image" src="https://github.com/user-attachments/assets/642cb488-8b91-4da0-a387-a46fa5037c06" />

``` js

let movers = [];

function setup() {
  createCanvas(640, 480);
  background(0); // para la estela
  for (let i = 0; i < 20; i++) {
    let x = random(width);
    let y = random(height / 2);
    let m = random(2, 8);
    movers.push(new Mover(x, y, m));
  }
  createP('Haz clic para aplicar viento a todas las bolitas.');
}

function draw() {
  // No usamos background para dejar estela, pero dibujamos un rectángulo semitransparente encima
  fill(0, 20); // blanco translúcido para que se vea la estela
  noStroke();
  rect(0, 0, width, height);

  for (let mover of movers) {
    let gravity = createVector(0, 0.2 * mover.mass);
    mover.applyForce(gravity);

    if (mover.contactEdge()) {
      let c = 0.05;
      let friction = mover.velocity.copy();
      friction.mult(-1);
      friction.setMag(c);
      mover.applyForce(friction);
    }

   if (mouseIsPressed) {
  // Viento aleatorio más fuerte para cada uno
  let wind = createVector(random(-3, 3), random(-3, 3));
  mover.applyForce(wind);
}

    mover.bounceEdges();
    mover.update();
    mover.show();
  }
}

class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 5;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.color = color(random(100, 255), random(100, 255), random(100, 255), 180);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    fill(this.color);
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }

  contactEdge() {
    return (this.position.y > height - this.radius - 1);
  }

  bounceEdges() {
    let bounce = -0.8;
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
  }
}

```

### Codigo de atraccion 

https://editor.p5js.org/isaacrisi/sketches/K4FiW1bmE

Modelé la fuerza como atracción gravitacional entre núcleo y electrón usando la ley de Newton, la fuerza depende de la masa y la distancia, simulando órbitas dinámicas. Conceptualmente representa sistemas atómicos donde los núcleos mantienen unidos a los electrones, y visualmente genera una obra generativa con patrones circulares, vibrantes y cambiantes. Al presionar espacio, cambia la masa de los núcleos, lo que altera la fuerza y modifica las órbitas, haciendo que el sistema evolucione de forma orgánica. Las trayectorias de los electrones reflejan la interacción entre masa, distancia y movimiento, formando una danza de partículas coloridas.

<img width="869" height="645" alt="image" src="https://github.com/user-attachments/assets/255dc329-d9a9-4759-b9da-99d943bd5003" />

``` js
let atoms = [];
let G = 5; // Constante gravitacional

function setup() {
  createCanvas(800, 600);
  noStroke();

  // Crear varios átomos
  for (let i = 0; i < 5; i++) {
    let x = random(100, width - 100);
    let y = random(100, height - 100);
    let mass = random(10, 30); // masa inicial del núcleo
    let col = color(random(100, 255), random(100, 255), random(100, 255));
    atoms.push(new Atom(x, y, mass, col, int(random(3, 6)))); // 3 a 5 electrones
  }
}

function draw() {
  background(20);

  for (let atom of atoms) {
    atom.update();
    atom.show();
  }
}

// Ahora la interacción es al presionar ESPACIO
function keyPressed() {
  if (key === ' ') { // barra espaciadora
    for (let atom of atoms) {
      atom.nucleus.mass = random(5, 50); // nueva masa
      atom.nucleus.radius = atom.nucleus.mass; // ajustar radio
    }
  }
}

// ---------------------
// CLASE ATOM
// ---------------------
class Atom {
  constructor(x, y, mass, col, numElectrons) {
    this.nucleus = new Attractor(x, y, mass, col);
    this.electrons = [];

    for (let i = 0; i < numElectrons; i++) {
      let angle = random(TWO_PI);
      let radius = random(30, 80);
      let ex = x + cos(angle) * radius;
      let ey = y + sin(angle) * radius;
      let electron = new Mover(ex, ey, random(0.5, 2), col);
      this.electrons.push(electron);
    }
  }

  update() {
    for (let e of this.electrons) {
      let force = this.nucleus.attract(e);
      e.applyForce(force);
      e.update();
    }
  }

  show() {
    this.nucleus.show();
    for (let e of this.electrons) {
      e.show();
    }
  }
}

// ---------------------
// CLASE ATTRACTOR (NÚCLEO)
// ---------------------
class Attractor {
  constructor(x, y, mass, col) {
    this.position = createVector(x, y);
    this.mass = mass;
    this.radius = mass;
    this.col = col;
  }

  attract(mover) {
    let force = p5.Vector.sub(this.position, mover.position);
    let distance = constrain(force.mag(), 5, 100);
    force.normalize();
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  show() {
    fill(this.col);
    circle(this.position.x, this.position.y, this.radius * 2);
  }
}

// ---------------------
// CLASE MOVER (ELECTRÓN)
// ---------------------
class Mover {
  constructor(x, y, mass, col) {
    this.mass = mass;
    this.radius = mass * 5;
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(2);
    this.acceleration = createVector(0, 0);
    this.col = col;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(-0.5);
  }

  show() {
    fill(this.col);
    circle(this.position.x, this.position.y, this.radius * 2);
  }
}

 ```


