# Unidad 3


## 🛠 Fase: Apply


- Explica cómo modelaste el problema de los n-cuerpos en tu obra.

  Modelé el problema de los n-cuerpos como un conjunto de partículas que se atraen entre sí mediante una fuerza parecida a la gravedad. Además, todas son atraídas por un "agujero negro" central que sigue al mouse. Para hacerlo más orgánico, les añadí una pequeña fuerza aleatoria basada en ruido de Perlin, como si flotaran en una energía cósmica. El resultado es una danza caótica pero armónica, inspirada en el universo de Lol, especificamente en Aurelion Sol.
  <img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/0dde5788-9ea7-47c2-b020-73cbeef67d2b" />

  
- Copia el enlace a tu simulación en p5.js.

  https://editor.p5js.org/isaacrisi/sketches/apc8Wj3CC 

- Copia el código.

  ``` js
    let movers = [];
    let sun;
    
    function setup() {
      createCanvas(800, 800);
      for (let i = 0; i < 300; i++) {
        let pos = p5.Vector.random2D();
        let vel = pos.copy();
        vel.setMag(random(2, 4));
        pos.setMag(random(200, 400));
        vel.rotate(PI / 2);
        let m = random(2, 6);
        movers[i] = new Mover(pos.x, pos.y, vel.x, vel.y, m);
      }
    
      sun = new Mover(0, 0, 0, 0, 1000); // Este seguirá al mouse
      background(10);
    }
    
    function draw() {
      // Fondo translúcido para efecto de trazo persistente
      background(10, 10);
    
      // Mover el centro al medio del canvas
      translate(width / 2, height / 2);
    
      // Actualizar posición del "sol" al mouse
      sun.pos = createVector(mouseX - width / 2, mouseY - height / 2);
    
      // Dibujar sol como un agujero negro
      noStroke();
      fill(0);
      ellipse(sun.pos.x, sun.pos.y, 20);
    
      for (let mover of movers) {
        sun.attract(mover);
        for (let other of movers) {
          if (mover !== other) {
            mover.attract(other);
          }
        }
      }
    
      for (let mover of movers) {
        mover.update();
        mover.show();
      }
    }
    
    class Mover {
      constructor(x, y, vx, vy, m) {
        this.pos = createVector(x, y);
        this.vel = createVector(vx, vy);
        this.acc = createVector(0, 0);
        this.mass = m;
        this.r = sqrt(this.mass) * 2;
        this.hue = random(200, 300); // color cósmico
        this.noiseOffset = createVector(random(1000), random(1000)); // posición para el ruido de Perlin
      }
    
      applyForce(force) {
        let f = p5.Vector.div(force, this.mass);
        this.acc.add(f);
      }
    
      attract(mover) {
        let force = p5.Vector.sub(this.pos, mover.pos);
        let distanceSq = constrain(force.magSq(), 100, 3000);
        let G = 1;
        let strength = (G * (this.mass * mover.mass)) / distanceSq;
        force.setMag(strength);
        mover.applyForce(force);
      }
    
      applyPerlinForce() {
        let angle = noise(this.noiseOffset.x, this.noiseOffset.y) * TWO_PI * 2;
        let mag = 0.05; // fuerza suave
        let force = p5.Vector.fromAngle(angle);
        force.setMag(mag);
        this.applyForce(force);
    
        // Avanzamos en el espacio de ruido para cambiar con el tiempo
        this.noiseOffset.add(0.01, 0.01);
      }
    
      update() {
        this.applyPerlinForce(); // aplicar el ruido antes de actualizar
        this.vel.add(this.acc);
        this.pos.add(this.vel);
        this.acc.set(0, 0);
      }
    
      show() {
        colorMode(HSB);
        noStroke();
        fill(this.hue, 255, 255, 0.6);
        ellipse(this.pos.x, this.pos.y, this.r * 2);
        colorMode(RGB);
      }
    }
  ```
- Captura una imagen representativa de tu ejemplo.
<img width="799" height="786" alt="image" src="https://github.com/user-attachments/assets/1dc5b20a-45cf-43a1-8e69-cf1a791721d8" />


