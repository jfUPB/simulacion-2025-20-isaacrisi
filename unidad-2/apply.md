# Unidad 2


## 🛠 Fase: Apply

### Actividad 8 

- Describe el concepto de tu obra generativa.

  para esta obra quisiera hacer como una especie de lienque que el usuario pueda ir pintando, quiero hacer una bola que se mueva con aceleracion hacia el mouse y vaya dejando una estela de color que sea como la pintura y que el usuario pueda pintar con esta, con un ruido de perlin quiero que el color vaya variando para que no se convierta en simplemente colorear la pantalla, el concepto de motion 101 va en el moviento de la bolita. 
  
- ¿Cómo piensas aplicar el marco MOTION 101 y por qué?

  Con el movimiento de la bolita
  
- ¿Qué algoritmo de aceleración vas a utilizar? ¿Por qué?

  Usare la aceleracion del mouse para generar la interactividad, pero hice que el factor de la gravedad hacia el mouse se formara a travez de la fucnion de la tangente hiperbolica
  
- El contenido generado debe ser interactivo. Puedes utilizar mouse, teclado, cámara, micrófono, etc, para variar los parámetros del algoritmo en tiempo real.
- El código de la aplicación.

  ```js
  let follower;
  let time = 0;
  
  function setup() {
    createCanvas(windowWidth, windowHeight);
    background(0);
    follower = new Follower();
  }
  
  function draw() {
    follower.update();
    follower.show();
    time += 0.01;
  }
  
  class Follower {
    constructor() {
      this.position = createVector(width / 2, height / 2);
      this.velocity = createVector();
      this.acceleration = createVector();
      this.topspeed = 5;
      this.colorOffset = 0;
    }
  
    update() {
      let mouse = createVector(mouseX, mouseY);
      let dir = p5.Vector.sub(mouse, this.position);
      dir.setMag(Math.tanh(time)*0.25); 
      this.acceleration = dir;
  
      this.velocity.add(this.acceleration);
      this.velocity.limit(this.topspeed);
      this.position.add(this.velocity);
    }
  
    show() {
  
      let r = pow(noise(this.colorOffset), 0.5) * 255;
      let g = pow(noise(this.colorOffset + 100), 0.5) * 255;
      let b = pow(noise(this.colorOffset + 200), 0.5) * 255;
  
      this.colorOffset += 0.02;
  
      noStroke();
      fill(r, g, b, 255);
      circle(this.position.x, this.position.y, 50);
    }
  }
  ```

  
- Un enlace al proyecto en el editor de p5.js.

  https://editor.p5js.org/isaacrisi/sketches/DnU913tNz 

- Una captura de pantalla representativa de tu pieza de arte generativo.

<img width="870" height="800" alt="image" src="https://github.com/user-attachments/assets/f4e99db2-2127-4be4-9c91-b02e62e657f2" />

