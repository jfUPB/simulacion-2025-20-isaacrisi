# Evidencias de la unidad 5

## Actividad 2

# 1er ejemplo 

En este caso cada partícula es creada, almacenada y actualizada hasta que su vida (lifespan) llega a 0. En ese momento, se elimina del arreglo. Esto evita que el sistema se sature, manteniendo la memoria bajo control.

El concepto aplicado en esta modificacion fue un levy flight en lugar de la aparcicion en random, lo que hace que algunas particulas aprezcan normalmente y otras volando.

En este sistema ya hay un control de particulas como dije en un inicio.

- Implementé Levy Flights usando una función levyFlightStep() basada en una distribución de potencia inversa.

- Sustituí la asignación aleatoria simple de la velocidad inicial por un vector generado por el flight.

- Verifiqué que las partículas ahora realicen movimientos largos esporádicos y erráticos, típicos del comportamiento Levy.

https://editor.p5js.org/isaacrisi/sketches/X19PpS1Bj

https://editor.p5js.org/natureofcode/sketches/-xTbGZMim

<img width="743" height="334" alt="image" src="https://github.com/user-attachments/assets/aeef6eab-c3dd-4370-94a6-021f474850cd" />

# 2do ejemplo 

En este caso el sistema de particulas se maneja de la misma forma que el anterior on el metodo del lifespan y el splice.

El concepto aplicado en esta modificacion fue el ruido de perlin para ver como la diferencia que se marcaba con el levy flight, viendo que ese componente agresivo se desaparece y se ve algo como mas armonico 

La variacion se hizo de la misma forma pero con un noise() en ligar de levyFlightStep().

https://editor.p5js.org/isaacrisi/sketches/v8BnzQ_EA

https://editor.p5js.org/natureofcode/sketches/2ZlNJp2EW

<img width="666" height="256" alt="image" src="https://github.com/user-attachments/assets/69ce5544-f56e-4fab-8da2-d12c18860419" />

# 3er ejemplo

Se crean partículas cada frame con addParticle(), que agrega aleatoriamente un Particle o un Confetti.

Cada partícula disminuye su lifespan en cada frame.

Cuando lifespan < 0, la partícula se elimina del array con splice().

Esto previene acumulación y mantiene la memoria bajo control.

Apliqué movimiento sinusoidal solo a los cuadrados (Confetti) modificando su posición x con una función sin() según el tiempo.

Esto genera un movimiento oscilante horizontal, simulando un vaivén natural y suave.

Se mantiene el movimiento vertical normal para caída/gravedad.

Solo los Confetti tienen este movimiento para diferenciar visualmente el comportamiento.

https://editor.p5js.org/natureofcode/sketches/2ZlNJp2EW

https://editor.p5js.org/isaacrisi/sketches/_PGH7otbV

<img width="558" height="287" alt="image" src="https://github.com/user-attachments/assets/d3b0ceb3-34ff-4a8c-a457-e353d9a19c1d" />


# 4to ejemplo 

Creación: Cada frame se añade una nueva partícula en la posición del emisor con addParticle().

Desaparición: Cada partícula tiene un lifespan que disminuye con el tiempo. Cuando es menor que 0, se elimina del array usando splice().

Memoria: Al eliminar partículas muertas, se libera memoria evitando que la lista crezca indefinidamente, manteniendo buen rendimiento.

Se aplica el concepto de motion 101 con aceleracion incluida

La aceleración, que antes era constante (gravedad), ahora es una fuerza aleatoria que cambia cada frame.

Esto genera un movimiento más orgánico e impredecible en las partículas.

https://editor.p5js.org/isaacrisi/sketches/hs5cSGpkx

https://editor.p5js.org/natureofcode/sketches/uZ9CfjLHL

<img width="614" height="418" alt="image" src="https://github.com/user-attachments/assets/cae5692f-bd3b-40db-be46-844a20720e6e" />

# 5to ejemplo

Cada frame se crean nuevas partículas en la posición del emisor, las partículas tienen un lifespan que disminuye hasta que mueren, las partículas muertas se eliminan del arreglo para liberar memoria, esto evita acumulación excesiva y mantiene el rendimiento.
 
En lugar de una fuerza de repulsión fija, ahora cada partícula es atraída hacia la posición actual del mouse.

Se calcula una fuerza que empuja a cada partícula lejos del mouse.

La fuerza es inversamente proporcional al cuadrado de la distancia, haciendo que la repulsión sea muy fuerte cuando la partícula está cerca.

Esto genera un efecto dinámico donde el mouse actúa como un “campo de fuerza” que repele las partículas.

https://editor.p5js.org/isaacrisi/sketches/m8PxYLQQl

https://editor.p5js.org/natureofcode/sketches/H4TMayNak

<img width="614" height="261" alt="image" src="https://github.com/user-attachments/assets/eb99f862-8b59-414f-a3cf-80ac0c4cf894" />


## aply 

Haciendo el ultimo ejercicio vi algo interesante y es que al poner el punto de repulsion como casi donde es el emisor de las particulas da la impresion de que esta disparando, por lo que quiero hacer la obra desde ese punto,  no pense como en un significado mas profundo que meramente diversion y ver algo que se podria ver interesante o incluso llevarse a un juego, como una especie de shooter ahi medio suave, voy a intentar hacer un arma que se pueda ir moviendo con el mouse y que se generen algunos cuadrados en el fondo que se puedan romper con o desaparecer al darles. 

<img width="697" height="534" alt="image" src="https://github.com/user-attachments/assets/c2300d1f-a744-4883-a234-13a027609310" />

https://editor.p5js.org/isaacrisi/sketches/zBiaNoGjM

´´´js

 let emitter;
 let boxes = [];
 let boxCount = 10;
 
 let shotType = 0; // Controla qué tipo de disparo usar
 let explosions = [];
 let backgroundParticles = [];
 
 class Particle {
   constructor(x, y) {
     this.position = createVector(x, y);
     this.velocity = createVector(0, -5);
     this.acceleration = createVector(0, 0);
     this.lifespan = 255;
   }
 
   applyForce(force) {
     this.acceleration.add(force);
   }
 
   update() {
     this.velocity.add(this.acceleration);
     this.position.add(this.velocity);
     this.lifespan -= 3;
     this.acceleration.mult(0);
   }
 
   show() {
     stroke(0, this.lifespan);
     fill(200, 50, 50, this.lifespan);
     ellipse(this.position.x, this.position.y, 8);
     // brillo sutil
     push();
     noStroke();
     fill(255, 150, 150, this.lifespan / 2);
     ellipse(this.position.x, this.position.y, 14);
     pop();
   }
 
   isDead() {
     return this.lifespan <= 0 || this.position.y < -10;
   }
 }
 
 class SinusoidalParticle extends Particle {
   constructor(x, y) {
     super(x, y);
     this.angle = random(TWO_PI);
     this.angleSpeed = 0.3;
     this.amplitude = 5;
   }
 
   update() {
     this.angle += this.angleSpeed;
     this.position.x += sin(this.angle) * this.amplitude;
     super.update();
   }
 
   show() {
     stroke(50, 100, 200, this.lifespan);
     fill(50, 150, 250, this.lifespan);
     ellipse(this.position.x, this.position.y, 10);
     // brillo sutil
     push();
     noStroke();
     fill(100, 200, 255, this.lifespan / 2);
     ellipse(this.position.x, this.position.y, 16);
     pop();
   }
 }
 
 class Box {
   constructor(x, y, size = 30) {
     this.position = createVector(x, y);
     this.size = size;
     this.isAlive = true;
     this.respawnTimer = 0;
   }
 
   show() {
     if (!this.isAlive) return;
     rectMode(CENTER);
     fill(150, 100, 200);
     stroke(50);
     strokeWeight(2);
     rect(this.position.x, this.position.y, this.size, this.size);
   }
 
   checkCollision(particle) {
     if (!this.isAlive) return false;
     return (particle.position.x > this.position.x - this.size / 2 &&
       particle.position.x < this.position.x + this.size / 2 &&
       particle.position.y > this.position.y - this.size / 2 &&
       particle.position.y < this.position.y + this.size / 2);
   }
 
   destroy() {
     this.isAlive = false;
     this.respawnTimer = 120;
     // Crear explosión de partículas
     for (let i = 0; i < 15; i++) {
       let angle = random(TWO_PI);
       let speed = random(1, 4);
       let velocity = p5.Vector.fromAngle(angle).mult(speed);
       explosions.push(new ExplosionParticle(this.position.x, this.position.y, velocity));
     }
   }
 
   update() {
     if (!this.isAlive) {
       this.respawnTimer--;
       if (this.respawnTimer <= 0) {
         this.respawn();
       }
     }
   }
 
   respawn() {
     this.position.x = map(noise(frameCount * 0.1 + random(1000)), 0, 1, 50, width - 50);
     this.position.y = random(50, height / 2);
     this.isAlive = true;
   }
 }
 
 class ExplosionParticle {
   constructor(x, y, velocity) {
     this.position = createVector(x, y);
     this.velocity = velocity;
     this.lifespan = 100;
   }
 
   update() {
     this.position.add(this.velocity);
     this.lifespan -= 6;
   }
 
   show() {
     noStroke();
     fill(255, 150, 50, this.lifespan);
     ellipse(this.position.x, this.position.y, 6);
   }
 
   isDead() {
     return this.lifespan <= 0;
   }
 }
 
 class BackgroundParticle {
   constructor() {
     this.position = createVector(random(width), random(height));
     this.size = random(1, 3);
     this.speed = random(0.2, 0.5);
     this.alpha = random(50, 100);
   }
 
   update() {
     this.position.y += this.speed;
     if (this.position.y > height) {
       this.position.y = 0;
       this.position.x = random(width);
     }
   }
 
   show() {
     noStroke();
     fill(200, 200, 255, this.alpha);
     ellipse(this.position.x, this.position.y, this.size);
   }
 }
 
 class Repeller {
   constructor(x, y, power) {
     this.position = createVector(x, y);
     this.power = power;
   }
 
   repel(particle) {
     let force = p5.Vector.sub(particle.position, this.position);
     let distance = force.mag();
     distance = constrain(distance, 5, 50);
     let strength = this.power / (distance * distance);
     force.setMag(strength);
     return force;
   }
 }
 
 class Emitter {
   constructor(x, y) {
     this.origin = createVector(x, y);
     this.particles = [];
     this.shootCooldown = 0;
     this.shootRate = 3; // más rápido disparo
     this.repeller = new Repeller(this.origin.x, this.origin.y + 10, 350); // repulsor justo debajo del emisor
   }
 
   updatePosition(x, y) {
     this.origin.set(x, y);
     this.repeller.position.set(x, y + 10);
   }
 
   shoot() {
     if (this.shootCooldown <= 0) {
       let p;
       if (shotType === 0) {
         p = new Particle(this.origin.x, this.origin.y);
       } else {
         p = new SinusoidalParticle(this.origin.x, this.origin.y);
       }
       this.particles.push(p);
       this.shootCooldown = this.shootRate;
     }
   }
 
   run() {
     for (let i = this.particles.length - 1; i >= 0; i--) {
       let p = this.particles[i];
 
       let repulseForce = this.repeller.repel(p);
       p.applyForce(repulseForce);
 
       p.update();
       p.show();
 
       for (let box of boxes) {
         if (box.isAlive && box.checkCollision(p)) {
           box.destroy();
           p.lifespan = 0;
         }
       }
 
       if (p.isDead()) {
         this.particles.splice(i, 1);
       }
     }
 
     if (this.shootCooldown > 0) this.shootCooldown--;
   }
 }
 
 function setup() {
   createCanvas(640, 480);
   emitter = new Emitter(width / 2, height - 30);
 
   for (let i = 0; i < boxCount; i++) {
     let x = map(noise(i * 0.5), 0, 1, 50, width - 50);
     let y = random(50, height / 2);
     boxes.push(new Box(x, y));
   }
 
   // Crear partículas de fondo
   for (let i = 0; i < 50; i++) {
     backgroundParticles.push(new BackgroundParticle());
   }
 }
 
 function draw() {
   background(20, 20, 30, 10 );
 
   // Mostrar y actualizar partículas de fondo
   for (let bp of backgroundParticles) {
     bp.update();
     bp.show();
   }
 
   emitter.updatePosition(constrain(mouseX, 20, width - 20), height - 30);
 
   for (let box of boxes) {
     box.update();
     box.show();
   }
 
   emitter.shoot();
   emitter.run();
 
   // Mostrar explosiones
   for (let i = explosions.length - 1; i >= 0; i--) {
     let e = explosions[i];
     e.update();
     e.show();
     if (e.isDead()) {
       explosions.splice(i, 1);
     }
   }
 
   // Dibujar el arma
   push();
   translate(emitter.origin.x, emitter.origin.y);
   fill(80, 80, 200);
   noStroke();
   triangle(-15, 20, 15, 20, 0, -20);
   pop();
 
   // Mostrar texto tipo de disparo
   fill(255);
   noStroke();
   textSize(16);
   text("Tipo de disparo: " + (shotType === 0 ? "Normal" : "Sinusoidal"), 10, height - 20);
 }
 
 function mousePressed() {
   // Cambia tipo de disparo con click
   shotType = (shotType + 1) % 2;
 }
´´´








