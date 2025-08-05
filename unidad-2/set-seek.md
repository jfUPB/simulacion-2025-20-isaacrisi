# Unidad 2

## 🔎 Fase: Set + Seek


### Actividad 1

- ¿Cómo funciona la suma dos vectores en p5.js?

  Se suma componente a componente
  
- ¿Por qué esta línea position = position + velocity; no funciona?

  Porque en esta biblioteca de javascript no hay sobrcarga para la suma por lo que no puede sumar vectores.

### Actividad 2

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
  this.position = createVector(width, height);
  this.speed = createVector(2,2.5);
  
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.position.x,this.position.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x+this.speed.x;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y+this.speed.y;
    } else {
      this.position.y--;
    }
  }
}
```

### Actividad 3

- El codigo va a copiuar en la consola primero el numero del vector original y luego el del valor modificado

- se obtuvo el resultado esperado

- En este codigo se pasa por referencia y aprendi que en este lenguaje solo se pueden hacer pasos por referencia, para poder hacer cambios en el valor sin modificar el vector original hay que crear una copia con la funcion copy()

### Actividad 4

- ¿Para qué sirve el método mag()?

  Sirve para saber la magnitud o el tamaño de un vector, como medir qué tan largo es.

- ¿Qué hace magSq() y cuál es la diferencia con mag()? ¿Cuál es más eficiente?

  magSq() te da el cuadrado de la magnitud, sin sacar raíz cuadrada. Es más rápido y eficiente que mag() porque evita esa operación costosa.

- ¿Para qué sirve el método normalize()?

  Sirve para hacer que un vector mantenga su dirección pero tenga magnitud 1, o sea, convertirlo en un vector unitario.

- ¿Para qué sirve el método dot()? (respuesta corta para un periodista curioso)

  Sirve para saber qué tanto apuntan dos vectores en la misma dirección.

- ¿Cuál es la diferencia entre la versión estática y la de instancia de dot()?

  La de instancia se usa como v1.dot(v2), la estática como PVector.dot(v1, v2). Hacen lo mismo, pero la estática no necesita que el primer vector ya exista.

- ¿Cuál es la interpretación geométrica del producto cruz?

  El producto cruz da un vector perpendicular a los dos vectores originales. Su dirección sigue la regla de la mano derecha (orientación), y su magnitud representa el área del paralelogramo formado por los dos vectores.

- ¿Para qué sirve el método dist()?

  Para calcular la distancia entre dos puntos o vectores, como si usaras una regla entre ellos.

- ¿Para qué sirven normalize() y limit()? (respuesta tipo memorizada):

  - normalize() sirve para que un vector siga apuntando igual pero con largo 1, muy útil para direcciones.

  - limit() sirve para que un vector no pase de cierta magnitud, por ejemplo para limitar la velocidad máxima.


### Actividad 5

- codigo

  https://editor.p5js.org/isaacrisi/sketches/vQI_uHi7b

- ¿Cómo funciona lerp() y lerpColor().

  Permiten encontrar un valor intermedio entre dos cosas en el caso de lerp fue con vectores, y en el lerp color es entre colores, la funcion recibe los dos extremos y un factor que es el que determina en que punto está entre los dos

- ¿Cómo se dibuja una flecha usando drawArrow()?

  Draw arrow recibe el punto inicial y el punto final de la flecha,  su color, y la dibuja asi.

### Actividad 6

- Geometricamente el motion funciona como una suma de vectores de un vector de posicion mas uno de velociada que hacen que un objeto se mueva de forma constante.

- En el ejemplo se usa para mover una bolita con dos vectores.

### Actividad 7

- Aceleración constante:

  El objeto empieza lento y cada vez se va moviendo más rápido, como cuando uno empieza a correr y va agarrando ritmo. Se siente parejo, sin sorpresas.

- Aceleración aleatoria:
  
  El objeto se mueve como si estuviera confundido o nervioso, cambiando de dirección todo el tiempo. Da una sensación de desorden, como si no supiera a dónde va.

- Aceleración hacia el mouse:

  El objeto parece tener vida propia y te quiere seguir. Se siente curioso, como si fuera un perrito que corre detrás tuyo, a veces con más fuerza, a veces más suave, dependiendo de qué tan lejos estés.

