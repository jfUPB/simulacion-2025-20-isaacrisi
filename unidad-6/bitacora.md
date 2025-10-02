# Evidencias de la unidad 6


## Actividad 1

<img width="118" height="70" alt="image" src="https://github.com/user-attachments/assets/1a970cad-db56-45a7-9f9f-468a63babb7c" />

Me impacta el contraste entre líneas oscuras y colores cálidos intensos que generan un movimiento profundo y rítmico.

<img width="70" height="70" alt="image" src="https://github.com/user-attachments/assets/c6e622a3-7b5e-462a-9aac-5e7076993b25" />

Me atrae la sutileza de las líneas suaves y la transición delicada entre vacío y densidad visual controlada.

Me inspira cómo combina reglas simples con variación orgánica para crear equilibrio entre lo técnico y lo artístico.

## Actividad 2 

- Qué es una fuerza de dirección (steering force)?

Es una fuerza que guía a un agente hacia un objetivo, ajustando su velocidad y dirección de forma suave.

- ¿Qué diferencia tiene con otras fuerzas en simulación de agentes?

Las fuerzas comunes impulsan directamente al agente; la steering force ajusta su movimiento actual, no lo reemplaza.

- ¿Qué relación tiene con Craig Reynolds?

Craig Reynolds propuso las steering forces para simular cómo se comportan animales en grupo, como pájaros o peces.

## Actividad 3

El campo de flujo se almacena como un array 2D donde cada celda contiene un vector que indica una dirección, y esos vectores se generan usando noise() para crear transiciones suaves entre direcciones vecinas. Un agente usa su posición para mapearla a una celda del campo, recupera el vector correspondiente y lo convierte en una fuerza de dirección restando su velocidad actual y limitando el resultado por maxforce. Los parámetros clave son la resolución del campo (tamaño de celda), la velocidad máxima (maxspeed) y la fuerza máxima (maxforce) del agente.

Modifiqué la generación inicial de los vehículos para que sus posiciones se distribuyeran siguiendo una distribución normal centrada en el medio del canvas en lugar de una distribución uniforme, lo que hizo que los agentes comenzaran agrupados en el centro y luego se dispersaran siguiendo el flujo del campo, generando un comportamiento más concentrado inicialmente y un movimiento colectivo que parte de un núcleo denso hacia el resto del espacio.

```js 

    function setup() {
      let text = createP(
        "Hit space bar to toggle debugging lines.<br>Click the mouse to generate a new flow field."
      );
      createCanvas(640, 240);
      flowfield = new FlowField(20);
  
    for (let i = 0; i < 120; i++) {
      // Generar con distribución normal centrada en el medio de la pantalla
      let x = constrain(randomGaussian(width / 2, width / 8), 0, width);
      let y = constrain(randomGaussian(height / 2, height / 8), 0, height);
      vehicles.push(new Vehicle(x, y, random(2, 5), random(0.1, 0.5)));
    }
    }
```

<img width="692" height="263" alt="image" src="https://github.com/user-attachments/assets/11de2cc9-4585-4f75-afd4-ee16be14afae" />

## Actividad 4

La separación busca evitar que los agentes se amontonen calculando un vector que los aleja de vecinos muy cercanos, la alineación intenta que los agentes coincidan su dirección con el promedio de la velocidad de vecinos cercanos, y la cohesión mueve a los agentes hacia la posición promedio de sus vecinos para mantener el grupo unido. Los parámetros clave son el radio de percepción que define el rango para considerar vecinos, los pesos que ajustan la influencia relativa de separación, alineación y cohesión, y la velocidad máxima y fuerza máxima de cada agente.

Se añadió una fuerza perpendicular al vector hacia el centro para que cada boid gire alrededor de ese punto, generando un movimiento circular colectivo y una línea rotando en grupo.

``` js
    orbit(center) {
      // Vector desde boid hacia el centro
      let toCenter = p5.Vector.sub(center, this.position);
      // Vector perpendicular para la fuerza de órbita
      let orbitForce = createVector(-toCenter.y, toCenter.x);
      orbitForce.normalize();
      orbitForce.mult(0.1); // Ajusta la intensidad de la órbita
      this.applyForce(orbitForce);
    }

```

``` js

flock(boids) {
  let sep = this.separate(boids);
  let ali = this.align(boids);
  let coh = this.cohere(boids);

  sep.mult(1.5);
  ali.mult(1.0);
  coh.mult(1.0);

  this.applyForce(sep);
  this.applyForce(ali);
  this.applyForce(coh);

  // Agregar fuerza para orbitar alrededor del centro de la pantalla
  let center = createVector(width / 2, height / 2);
  this.orbit(center);
}


```
https://github.com/user-attachments/assets/4f70cb86-5745-4866-b394-fcc6b529f38c


## Actividad 4

### 1. Reglas del Flocking

- Separación (Separation)

- Objetivo: Evitar que los agentes se amontonen o choquen entre sí.

- Lógica: Cada boid calcula un vector que apunta en dirección contraria a los vecinos demasiado cercanos. Entre más cerca esté un vecino, más fuerte es el empuje de alejamiento.

- Alineación (Alignment)

- Objetivo: Hacer que los boids se muevan en una dirección similar, como si siguieran la corriente del grupo.

- Lógica: El boid promedia las velocidades de sus vecinos y genera un vector que lo empuja a alinearse con esa dirección promedio.

- Cohesión (Cohesion)

- Objetivo: Mantener al grupo unido sin que se disperse demasiado.

- Lógica: El boid calcula la posición promedio de sus vecinos cercanos y genera un vector que lo dirige hacia ese centro, como si buscara mantenerse dentro del “enjambre”.

### 2. Parámetros clave identificados

- Radio de percepción:

desiredSeparation = 25 (para separación).

neighborDistance = 50 (para alineación y cohesión).

- Pesos de las reglas:

Separación: 1.5

Alineación: 1.0

Cohesión: 1.0

- Velocidad y fuerza máximas:

maxspeed = 3

maxforce = 0.05

### 3. Modificación y efectos observados

Modificación realizada: Se aumentó de forma drástica el peso de la regla de Separación y se redujo la Cohesión a cero.

Efecto observado:
El comportamiento colectivo cambió notablemente. Los boids comenzaron a evitarse mucho entre sí, lo que generó un enjambre mucho más disperso y desordenado. Ya no se formaban grupos compactos, sino que los agentes mantenían grandes distancias y se movían de manera caótica por toda la pantalla.
La ausencia de cohesión eliminó la tendencia a reagruparse, así que cada boid terminó siguiendo más su propia ruta, solo corrigiendo su movimiento para no acercarse demasiado a otros.

### 4. Fragmento de código modificado

Dentro del método flock(boids) de la clase Boid, cambié los multiplicadores de cada fuerza:

``` js

    flock(boids) {
      let sep = this.separate(boids); // Separation
      let ali = this.align(boids);    // Alignment
      let coh = this.cohere(boids);   // Cohesion
    
      // Ajuste de pesos
      sep.mult(3.0);   // antes 1.5 → mucho más fuerte
      ali.mult(1.0);   // igual que antes
      coh.mult(0.0);   // antes 1.0 → eliminado
    
      // Aplicar fuerzas
      this.applyForce(sep);
      this.applyForce(ali);
      this.applyForce(coh);
    }
}

```

<img width="635" height="247" alt="image" src="https://github.com/user-attachments/assets/079e49b3-70f3-4b05-9fc3-1464b05169d1" />

# Apply


##  Concepto  
La idea fue crear una experiencia audiovisual inmersiva donde las **estrellas** se mueven en un **campo de flujo** invisible, respondiendo al ritmo de la canción *Golden*.  
El objetivo: pasar de un ambiente **misterioso y morado** hacia un **clímax dorado y luminoso**.  

##  Decisiones de diseño  
- **Formas**: se eligieron estrellas en lugar de triángulos, reforzando la sensación cósmica.  
- **Paleta**: transición de **morado oscuro → dorado cálido**, con fondo negro/morado que cambia suavemente con los beats.  
- **Sonido**: el beat afecta velocidad y color, logrando sincronía entre música y visuales.  
- **Movimiento**: el flujo genera trayectorias orgánicas, como energía en constante transformación.  

##  Proceso  
1. Bocetos en papel de campos de flujo y variaciones de formas.  
2. Pruebas de paleta: se descartaron azules en favor de morados profundos.  
3. Ajustes visuales con beats para dar vida a la pieza.  

## Resultado  
Una visualización dinámica donde las estrellas viajan y cambian de color con la música.  
El inicio es **oscuro y etéreo**, y el final se llena de **energía dorada**, representando el viaje emocional de la canción.  



https://editor.p5js.org/isaacrisi/sketches/ks4seTPCy

``` js

// ==== Variables globales y estructuras base (FlowField + Vehicle) adaptadas ====
let flowfield;
let vehicles = [];
let debugUI = true;
let cols, rows;

// Audio
let song;
let fft, amp;
let beatThreshold = 0.02;
let sens = 1.0;
let lastBeatTime = 0;
let beatHold = 180; // ms entre beats mínimos
let pulse = 0;
let fileInput, playBtn, debugChk, sensSlider, threshSlider;

let controller;

let c1, c2;

let amplitude;
let waves = [];



// 🎨 Fondo
let bgBase, bgVariation = 0;

function setup() {
  createCanvas(windowWidth, windowHeight);
  flowfield = new FlowField(30);
 amplitude = new p5.Amplitude();
 
  // Vehículos
  for (let i = 0; i < 220; i++) {
    let ms = random(1.2, 3.5);
    let mf = random(0.05, 0.25);
    vehicles.push(new Vehicle(random(width), random(height), ms, mf));
  }

  // Controlador al centro
  controller = createVector(width/2, height/2);

  // Audio / FFT
  fft = new p5.FFT(0.8, 256);
  amp = new p5.Amplitude();

  // UI
  fileInput = select('#file');
  playBtn = select('#playbtn');
  debugChk = select('#debug');
  sensSlider = select('#sens');
  threshSlider = select('#threshold');

  playBtn.mousePressed(togglePlay);
  fileInput.changed(handleFile);
  debugChk.changed(()=> debugUI = debugChk.elt.checked);
  sensSlider.input(()=> sens = float(sensSlider.value()));
  threshSlider.input(()=> beatThreshold = float(threshSlider.value()));

  select('#status').html('Listo. Carga un archivo de audio.');

  // 🎨 color base morado oscuro
  colorMode(HSB, 360, 100, 100);
  bgBase = color(270, 60, 8); // morado casi negro
  c1 = color(360, 80, 80);  // morado más cálido (hacia magenta)
  c2 = color(255, 215, 0);   // dorado/amarillo
}

function windowResized(){
  resizeCanvas(windowWidth, windowHeight);
  flowfield = new FlowField(30);
}

function draw() {
  // === mover controlador con WASD ===
  let speed = 4;
  if (keyIsDown(87)) controller.y -= speed; // W
  if (keyIsDown(83)) controller.y += speed; // S
  if (keyIsDown(65)) controller.x -= speed; // A
  if (keyIsDown(68)) controller.x += speed; // D

  // limitar dentro del canvas
  controller.x = constrain(controller.x, 0, width);
  controller.y = constrain(controller.y, 0, height);

  // === perturbamos el FlowField cerca del controlador ===
  flowfield.disturb(controller, 80, 0.25);  

  // === 🎨 Fondo dinámico ===
  // morado inicial y dorado final según progreso
  let t = 0;
  if (song && song.isLoaded()) {
    t = constrain(song.currentTime() / song.duration(), 0, 1);
  }
  let c1 = color(270, 60, 15); // morado oscuro
  let c2 = color(45, 80, 60);  // amarillo/dorado apagado
  let bg = lerpColor(c1, c2, easeInOutCubic(t));

  // aplicar variación por beat
  let h = hue(bg) + bgVariation;
  let s = saturation(bg) + randomGaussian() * 2;
  let b = brightness(bg) + randomGaussian() * 3;
  h = constrain(h, 250, 280); // controlado hacia morados
  s = constrain(s, 40, 90);
  b = constrain(b, 5, 25);

  background(h, s, b, 10);

  // Análisis de audio
  let level = 0;
  let specLevel = 0;
  if (song && song.isPlaying()) {
    level = amp.getLevel();
    let spectrum = fft.analyze();
    specLevel = fft.getEnergy(200, 2000) / 255.0;
  }

  // Detección de beats
  let now = millis();
  if (level * sens > beatThreshold && (now - lastBeatTime) > beatHold) {
    lastBeatTime = now;
    pulse = map(level * sens, beatThreshold, 0.2, 0.8, 2.2);
    pulse = constrain(pulse, 0.6, 2.8);

    // ⚡ variación extra al fondo en cada beat
    bgVariation = randomGaussian() * 8;

    for (let v of vehicles) {
      v.maxspeed = v.baseMaxspeed * pulse;
    }
     // medir energía de la canción
  let levels = amplitude.getLevel();

  // umbral para detectar "golpe"
  let threshold = 0.1;
  if (levels > threshold && frameCount % 5 === 0) { 
    // nueva onda desde el centro
    waves.push(new Wave(width/2, height/2));
  }

  // dibujar ondas activas
  for (let i = waves.length - 1; i >= 0; i--) {
    waves[i].update();
    waves[i].show();
    if (waves[i].finished()) {
      waves.splice(i, 1); // eliminar cuando se desvanece
    }
  }
  }

  // Decaimiento del pulso
  bgVariation = lerp(bgVariation, 0, 0.1);
  for (let v of vehicles) {
    v.maxspeed = lerp(v.maxspeed, v.baseMaxspeed, 0.06);
  }

  // Modulamos magnitud del campo
  flowfield.setMagnitude(map(specLevel, 0, 1, 0.4, 1.8));

  // Mostrar el campo (si debug activado)
  // if (debugUI) flowfield.show();

  // Vehículos
  for (let v of vehicles) {
  v.follow(flowfield);
  v.run();

  let speedT = map(v.velocity.mag(), 0, v.baseMaxspeed*1.8, 0, 1);
  // Mezcla entre morado (c1) y dorado (c2)
  let vehicleColor = lerpColor(c1, c2, constrain(t*0.9 + speedT*0.25, 0, 1));
  
  // 🔥 Usar directo el color con alpha
  vehicleColor.setAlpha(200);
  v.displayColor = vehicleColor;
}


  // UI tiempo
  if (song && song.isLoaded()) {
    select('#time').html(formatTime(song.currentTime()) + ' / ' + formatTime(song.duration()));
    select('#status').html(song.isPlaying() ? 'Reproduciendo' : 'Pausado');
  } else {
    select('#time').html('0:00 / 0:00');
  }
}


// ===== FlowField (similar al original, con control de magnitud) =====
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.magnitude = 1.0;
    this.init();
  }

  init() {
    noiseSeed(floor(random(10000)));
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.18; // escala del ruido para permitir patrones grandes
      }
      xoff += 0.18;
    }
  }

  setMagnitude(m) { this.magnitude = m; }

  show() {
    strokeWeight(1);
    stroke(255, 80);
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let w = width / this.cols;
        let h = height / this.rows;
        let v = this.field[i][j].copy();
        v.setMag(w * 0.5 * this.magnitude);
        let x = i * w + w / 2;
        let y = j * h + h / 2;
        push();
        translate(x,y);
        line(0,0,v.x,v.y);
        pop();
      }
    }
  }
       disturb(pos, radius, strength) {
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let x = i * this.resolution + this.resolution/2;
        let y = j * this.resolution + this.resolution/2;
        let d = dist(pos.x, pos.y, x, y);
        if (d < radius) {
          // dirección de alejamiento
          let dir = p5.Vector.sub(createVector(x,y), pos);
          dir.normalize();
          dir.rotate(random(-0.3,0.3)); // un poco de caos
          dir.mult(strength);
          this.field[i][j].add(dir);
          this.field[i][j].normalize(); // mantener unitario
        }
      }
    }
  }


  lookup(position) {
    let column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
    let row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

// ===== Vehicle (basado en Flow Follow) =====
class Vehicle {
  constructor(x, y, ms, mf) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = p5.Vector.random2D();
    this.r = random(2.5, 5.0);
    this.baseMaxspeed = ms;
    this.maxspeed = ms;
    this.maxforce = mf;
    this.displayColor = color(200);
  }

  run() {
    this.update();
    this.borders();
    this.show();
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed * 0.9);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
    // pequeño jitter aleatorio para evitar simetrías exactas
    let j = p5.Vector.random2D().mult(0.005);
    this.applyForce(j);
  }

  applyForce(f) { this.acceleration.add(f); }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed * 1.4);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  show() {
  fill(this.displayColor);
  noStroke();
  push();
  translate(this.position.x, this.position.y);
  // opcional: rotación según dirección
  rotate(this.velocity.heading());
  // dibuja una estrella (radio interno, radio externo, nº de puntas)
  star(0, 0, this.r, this.r*2, 5);
  pop();
}

}

// ===== Helpers: audio control, file handling, format time =====
function handleFile(evt) {
  let file = evt.target.files[0];
  if (!file) return;
  if (song && song.isPlaying()) song.stop();
  if (song) song.disconnect();
  song = loadSound(URL.createObjectURL(file), ()=>{
    fft.setInput(song);
    amp.setInput(song);
    select('#status').html('Audio cargado');
    song.play();
  }, (err)=>{
    console.error(err);
    select('#status').html('Error cargando audio');
  });
}

function togglePlay() {
  if (!song) {
    select('#status').html('Selecciona un archivo con "Cargar audio"');
    return;
  }
  if (song.isPlaying()) {
    song.pause();
  } else {
    song.loop();
  }
}

function formatTime(sec) {
  if (!sec || isNaN(sec)) return '0:00';
  let s = floor(sec % 60);
  let m = floor(sec / 60);
  return nf(m,1) + ':' + nf(s,2);
}

// Simple easing para la transición de color
function easeInOutCubic(x) {
  return x < 0.5 ? 4*x*x*x : 1 - pow(-2*x+2,3)/2;
}

function star(x, y, radius1, radius2, npoints) {
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

class Wave {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.radius = 0;
    this.alpha = 200;
  }

  update() {
    this.radius += 5;       // velocidad de expansión
    this.alpha -= 3;        // desvanecimiento
  }

  finished() {
    return this.alpha <= 0;
  }

  show() {
    noFill();
    stroke(200, 170, 0, this.alpha); // amarillo oscuro
    strokeWeight(3);
    ellipse(this.x, this.y, this.radius*2);
  }
}



```

<img width="840" height="760" alt="image" src="https://github.com/user-attachments/assets/8ab615d6-c859-4c97-b090-ccec5046c374" />


Calificacion 

5: realicé las 5 actividades completas y la autoevaluación.

Para todas las actividades hice todo lo propuesto, investigue y tuve un muy buen trabajo en general entendiendo a profundidad todos los conceptos de la unidad por eso la nota propuesta es 5.0








