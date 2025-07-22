# Unidad 1

## 🛠 Fase: Apply

### Actividad 8

lo que tengo en mente para hacer es una obra donde el mouse sea como un centro de gravedad o algo similar que por donde pase cambie esa zona, voy a usar distribucion normal para que los colores queden por zonas cercanas al rojo y al negro, el ruido de perlin para que sean movimientos como sueaves y controlados, y usando el salto de levy para que hayan saltos largos y caoticos, cambiando lo que hace el ruido de perlin, esta obra busca representar de forma abstracta del flujo emocional y corporal que acompaña al periodo menstrual

codigo 

'''
    let walkers = [];
    let cyclePhase = 0;
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      background(0);
      noStroke();
    
      for (let i = 0; i < 300; i++) {
        walkers.push(new Walker());
      }
    }
    
    function draw() {
      // Fondo oscuro que se regenera suavemente
      fill(0, 10);
      rect(0, 0, width, height);
    
      // Avanza el ciclo (de 0 a TWO_PI repetidamente)
      cyclePhase += 0.005;
      if (cyclePhase > TWO_PI) cyclePhase = 0;
    
      for (let w of walkers) {
        w.update(cyclePhase);
        w.display(cyclePhase);
      }
    }
    
    class Walker {
      constructor() {
        this.pos = createVector(random(width), random(height));
        this.prevPos = this.pos.copy();
        this.tx = random(1000);
        this.ty = random(1000);
        this.alpha = random(30, 100);
        this.radius = random(8, 30);
        this.recentlyJumped = false;
      }
    
      update(phase) {
        this.prevPos = this.pos.copy();
        this.recentlyJumped = false;
    
        let cycleIntensity = sin(phase); // Valor oscilante entre -1 y 1
    
        if (random() < map(cycleIntensity, -1, 1, 0.005, 0.05)) {
          // 💥 Salto Lévy: representa un espasmo o ruptura emocional
          let angle = random(TWO_PI);
          let stepSize = this.levyStep() * map(cycleIntensity, -1, 1, 0.5, 1.5);
          let jump = p5.Vector.fromAngle(angle).mult(stepSize);
          this.pos.add(jump);
          this.recentlyJumped = true;
        } else {
          // 🌀 Movimiento suave tipo niebla
          let angle = noise(this.tx, this.ty) * TWO_PI * 4;
          let dir = createVector(cos(angle), sin(angle)).mult(1.5);
          this.pos.add(dir);
        }
    
        this.tx += 0.005;
        this.ty += 0.005;
    
        if (
          this.pos.x < -100 ||
          this.pos.x > width + 100 ||
          this.pos.y < -100 ||
          this.pos.y > height + 100
        ) {
          this.pos = createVector(random(width), random(height));
          this.tx = random(1000);
          this.ty = random(1000);
        }
      }
    
      display(phase) {
        let d = dist(this.pos.x, this.pos.y, mouseX, mouseY);
    
        // Color visceral: rojo profundo + violeta, basado en distancia al mouse
        let g = randomGaussian(d, 60);
        g = constrain(g, 0, 255);
        let cycleRed = map(sin(phase), -1, 1, 120, 255); // Pulso rojo cíclico
        let r = map(g, 0, width / 2, cycleRed, 20);
        let b = map(g, 0, width / 2, 60, 100); // toque púrpura
        let col = color(r, 0, b, this.alpha);
    
        fill(col);
        ellipse(this.pos.x, this.pos.y, this.radius, this.radius);
    
        // 🌩️ Efecto más grande si acaba de saltar
        if (this.recentlyJumped) {
          fill(255, 30); // Pulso blanco suave
          ellipse(this.pos.x, this.pos.y, this.radius * 2.5);
        }
      }
    
      levyStep() {
        let beta = 1.5;
        let u = random(-1, 1) * pow(1 - random(), -1 / beta);
        return constrain(u * 60, -200, 200);
      }
    }
'''
