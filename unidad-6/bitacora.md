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











