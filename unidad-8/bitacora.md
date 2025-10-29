# Evidencias de la unidad 8

## Performances observadas
- Sónar+D CCCB 2020 – Carles Viarnès & Alba G. Corral (360º AV Show)  
- Le Parody & Alba G. Corral – Teatro Principal de Zaragoza  
- (Exploración adicional del blog de Alba G. Corral)

---

## Conexión Sonido–Imagen

### Sónar+D CCCB 2020
Los visuales reaccionan al ritmo y la atmósfera del piano y los sintetizadores.  
Las formas parecen expandirse con los graves y brillar con los agudos, creando una sensación envolvente.  
El color y la luz acompañan los cambios de intensidad emocional de la música.

### Le Parody & Alba G. Corral
Aquí la conexión es más rítmica y tribal.  
Los visuales siguen el pulso electrónico y los golpes de percusión, generando explosiones visuales y flujos que se sincronizan con la voz.  
Hay un diálogo constante entre el movimiento visual y la energía sonora.

---

## Elementos Generativos

- Formas que evolucionan orgánicamente sin repetirse.  
- Texturas dinámicas que aparecen y desaparecen al ritmo de la música.  
- Sensación de que cada presentación sería única, ya que los algoritmos visuales parecen responder en tiempo real.  
- Los patrones no son fijos, sino generativos, como si “vivieran” junto a la música.

---

## Reflexión sobre la “Liveness”

La interacción en tiempo real transmite presencia y espontaneidad.  
Se percibe que la artista está “tocando” con luz y color, no solo mostrando un video.  
Esa imprevisibilidad genera una experiencia viva, donde sonido e imagen respiran juntos.  
Cada performance se siente como un instante irrepetible.

# Actividad 2

Cancion: [https://youtu.be/pl2K9rvsS74 ](https://youtu.be/pl2K9rvsS74) 

Paint the town blue (ashiniko) 

Quiero seguir como mucho el estilo de lo arcano y de la parte tambien de zaun con los jinxers como por un lado esa parte de la magia con lo arcano y las runas salvajes y por otro lado el humo y lo agresivo de los jinxers.

quiero manjear mucho cambios de color, de forma tamaños y los flowfields que voy a manejar 

flow fields, particulas y motion 101


Pieza 

``` js
let particles = [];
let flowField = [];
let cols, rows;
let inc = 0.1;
let zoff = 0;
let scl = 20;

let song;
let amp;
let fft;
let energy = 0;
let smokePuffs = [];

let beatThreshold = 300; // umbral inicial
let smokeAmount = 3;     // cantidad de nubes por beat

function preload() {
  song = loadSound("paint_the_town_blue.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 255);
  background(0);

  cols = floor(width / scl);
  rows = floor(height / scl);

  amp = new p5.Amplitude();
  fft = new p5.FFT(0.8, 64);

  for (let i = 0; i < 1200; i++) {
    particles[i] = new Particle();
  }

  userStartAudio();
}

function draw() {
  fill(0, 5);
  noStroke();
  rect(0, 0, width, height);

  fft.analyze();
  energy = fft.getEnergy("bass");
  let ampLevel = amp.getLevel();
  let audioForce = map(energy, 0, 255, 0.3, 2.0);

  // Campo de flujo dinámico
  let yoff = 0;
  for (let y = 0; y < rows; y++) {
    let xoff = 0;
    for (let x = 0; x < cols; x++) {
      let index = x + y * cols;
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 4;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(0.4 + audioForce);
      flowField[index] = v;
      xoff += inc;
    }
    yoff += inc;
  }
  zoff += 0.002;

  // Spray con mouse
  if (mouseIsPressed) {
    let blueColor = color(170, 255, 255, 180);
    sprayPaint(mouseX, mouseY, 300, blueColor);
  }

  // --- Nubes de humo con el beat ---
  if (energy > beatThreshold) {
    for (let i = 0; i < smokeAmount; i++) { // 💨 control dinámico
      let hueBase = random(160, 190);
      let c = color(hueBase, random(150, 255), 255, 80);
      smokePuffs.push(new Smoke(random(width), random(height), c));
    }
  }

  // Dibujar nubes
  for (let i = smokePuffs.length - 1; i >= 0; i--) {
    smokePuffs[i].update();
    smokePuffs[i].show();
    if (smokePuffs[i].finished()) {
      smokePuffs.splice(i, 1);
    }
  }

  // Dibujar partículas
  for (let p of particles) {
    p.follow(flowField);
    p.update();
    p.edges();
    p.show();
  }

  // Mostrar valores
  fill(255);
  textSize(16);
  text(`Umbral: ${beatThreshold}`, 20, 30);
  text(`Cantidad de humo: ${smokeAmount}`, 20, 55);
}

// --- Clase de partícula ---
class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxspeed = 2;
    this.h = random(150, 255);
  }

  follow(vectors) {
    let x = floor(this.pos.x / scl);
    let y = floor(this.pos.y / scl);
    let index = x + y * cols;
    let force = vectors[index];
    if (force) this.applyForce(force);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxspeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    stroke(this.h, 255, 255, 90);
    strokeWeight(1.6);
    point(this.pos.x, this.pos.y);
    this.h += 0.4;
    if (this.h > 255) this.h = 0;
  }

  edges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }
}

// --- Clase de humo ---
class Smoke {
  constructor(x, y, col) {
    this.pos = createVector(x, y);
    this.col = col;
    this.alpha = 255;
    this.size = random(80, 200);
    this.vel = createVector(random(-0.3, 0.3), random(-0.5, 0.1));
  }

  update() {
    this.pos.add(this.vel);
    this.alpha -= 2.5;
  }

  finished() {
    return this.alpha <= 0;
  }

  show() {
    push();
    noStroke();
    fill(hue(this.col), saturation(this.col), brightness(this.col), this.alpha / 2);
    for (let i = 0; i < 10; i++) {
      let offset = p5.Vector.random2D().mult(random(this.size * 0.3));
      ellipse(this.pos.x + offset.x, this.pos.y + offset.y, this.size * random(0.4, 1.0));
    }
    pop();
  }
}

// --- Spray ---
function sprayPaint(x, y, density, col) {
  push();
  blendMode(ADD);
  fill(col);
  noStroke();
  for (let i = 0; i < density; i++) {
    let angle = random(TWO_PI);
    let radius = abs(randomGaussian(15, 8));
    let px = x + cos(angle) * radius;
    let py = y + sin(angle) * radius;
    circle(px, py, random(2, 4));
  }
  pop();
  blendMode(BLEND);
}

// --- Controles ---
function keyPressed() {
  // Subir / bajar el umbral
  if (key === 'ArrowUp') beatThreshold = min(305, beatThreshold + 15);
  if (key === 'ArrowDown') beatThreshold = max(0, beatThreshold - 15);

  // Cambiar cantidad de humo
  if (key === 'ArrowRight') smokeAmount = min(20, smokeAmount + 1);
  if (key === 'ArrowLeft') smokeAmount = max(1, smokeAmount - 1);

  // Limpiar pantalla
  if (key === 'r' || key === 'R') {
    push();
    noStroke();
    fill(0);
    rect(0, 0, width, height);
    pop();
  }

  // Play / Pause
  if (key === 'p' || key === 'P') {
    if (song.isPlaying()) {
      song.pause();
    } else {
      song.loop();
    }
  }

  console.log(`Umbral: ${beatThreshold} | Humo: ${smokeAmount}`);
}
```
https://editor.p5js.org/isaacrisi/sketches/sKaAfeeik

<img width="835" height="745" alt="image" src="https://github.com/user-attachments/assets/ab2e8634-f3ab-4e82-b7b6-0e6f0c0f599d" />

<img width="403" height="331" alt="image" src="https://github.com/user-attachments/assets/44657c09-9ca8-4266-8195-f495787bee94" />
<img width="827" height="737" alt="image" src="https://github.com/user-attachments/assets/63cfd74d-af15-4d21-8e42-556834f175e5" />







