# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1

Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.

La aleatoriedad permite que a pesar de que se realice muchas veces la misma obra, no se va a ver igual.

### Actividad 2 

Luego de ver el trabajo de Sofía piensa y escribe en TUS PROPIAS palabras:

- Cuál es el papel de la aleatoriedad en su obra.

  La aleatoriedad en esta obra permite que se distribuya de una forma siempre distinta las lineas que se van creando dando un aire distinto cada que se reproduce sin perder el objetivo principal.
  
- Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.

  dentro de mi perfil que me gustan mas las partes tecnicas en la animacion, como rigging o sfx, se pueden aplicar en sistemas de particulas o para simulaciones fisicas de agua, viento, cabello o ropa.

### Actividad 3

Realiza el siguiente experimento y reporta los resultados en tu bitácora:

  - Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
    
    https://editor.p5js.org/isaacrisi/sketches/t2WMEZnHi
    
  - Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
    
    dentro de la funcion step encontre algo creo maneja que tan grande se ve lo aleatorio y queria que se viera como un rayo por lo que modifique lo que aumentan las variables de 1 a 5 para ver que tal y tambien quiero que empiece desde arriba de la pantalla
    
  - Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
    
    <img width="639" height="258" alt="image" src="https://github.com/user-attachments/assets/fbe42cca-97fc-49e0-87a8-7cf0de04978b" />

  - Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
    
    mas o menos si ocurrio lo que queria, se ve como un rayo pero en lugar de empezar desde arriba empezo desde abajo por la forma en que se calcula, lo que tuve que hacer era que las variables iniciaran en 0,0 y no en width,height y tambien deberia de hacer que   arranque a crecer hacia arriba.
    
  
### Actividad 4

  - En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
    
    En una distribucion uniforme todos los numeros tienen la misma probabilidad de ser escogidos, por ejemplo en un dado justo todos los numeros tienen 1/6 de probabilidades, mientras que en una no uniforme las probabilidades varian como lo es en la distribucion normal donde los valore mas cercanos a la media son mas probables que los que se alejan.
    
  - Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.
    
    https://editor.p5js.org/isaacrisi/sketches/t2WMEZnHi

### Actividad 5

  - <img width="369" height="297" alt="image" src="https://github.com/user-attachments/assets/794bd144-44c9-4105-9896-18bdbc879988" />

  - <img width="419" height="338" alt="image" src="https://github.com/user-attachments/assets/325faa08-5d89-4ad3-b518-a7529d9fd959" />

  - <img width="339" height="340" alt="image" src="https://github.com/user-attachments/assets/f1e156e7-3f0f-44c7-aaa4-e9dab99118c1" />

  https://editor.p5js.org/isaacrisi/sketches/fIQhJbnr8

### Actividad 6

  - Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight. +
    
  - Explica por qué usaste esta técnica y qué resultados esberabas obtener.
    
    Esperaba hacer una especie de depedador que se mueve poco en su espacio y despues da saltos grandes pero con poca frecuencia hacia la presa lo imagine como un tiburon o algo asi, por eso el fondo azul.

  - Copia el código en tu bitácora.
   
    ´´´java script
    
        let pos;
      
        function setup() {
          createCanvas(800, 600);
          background(0);
          pos = createVector(width/2, height/2);
          stroke(255);
          strokeWeight(2);
        }
        
        function draw() {
          // Movimiento Lévy: la mayoría de los pasos son pequeños, algunos son grandes
          let step = p5.Vector.random2D();
          step.mult(random(1) < 0.01 ? random(100, 200) : random(1, 5));
          
        let newPos = p5.Vector.add(pos, step);
        line(pos.x, pos.y, newPos.x, newPos.y);
        pos = newPos;
      
        // Evita que el punto se salga del lienzo
        pos.x = constrain(pos.x, 0, width);
        pos.y = constrain(pos.y, 0, height);
      }
    
    ´´´
  - Coloca en enlace a tu sketch en p5.js en tu bitácora.
    
    https://editor.p5js.org/isaacrisi/sketches/BI3D03NEc
    
  - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

    <img width="670" height="358" alt="image" src="https://github.com/user-attachments/assets/7e7d7755-b7aa-4bb4-99d3-ff9c81b5b71f" />

    
### Actividad 7

  - Una vez has entendido el concepto de ruido Perlin, vas a pensar en una nueva manera de visualizarlo.

  - Crea un nuevo sketch en p5.js donde los visualices.

  - Explica el concepto qué resultados esberabas obtener.

   Buscaba usar el ruido Perlin para mover los colores de fondo de forma suave. A diferencia del random, que cambia bruscamente, el noise() hace que todo se vea más fluido y bonito, el ruido perlin permite que sea todo de forma mas suave como se nota en el codigo.

  - Copia el código en tu bitácora.

´´´javascript

    let xoff = 0;
    let hueRandom = 0;
    
    function setup() {
      createCanvas(600, 400);
      colorMode(HSB, 255);
      noStroke();
      frameRate(10); // Para que los cambios se noten bien
    }
    
    function draw() {
      // --- Lado izquierdo: ruido Perlin ---
      let hueNoise = noise(xoff) * 255;
      fill(hueNoise, 200, 255);
      rect(0, 0, width / 2, height);
    
      // --- Lado derecho: random puro ---
      hueRandom = random(255);
      fill(hueRandom, 200, 255);
      rect(width / 2, 0, width / 2, height);
    
      // Texto descriptivo
      fill(0);
      textSize(20);
      textAlign(CENTER, CENTER);
      text("Perlin noise", width / 4, height - 30);
      text("Random", (3 * width) / 4, height - 30);
    
      // Aumentar el valor de xoff poco a poco
      xoff += 0.01;
    }
  ´´´


  - Coloca en enlace a tu sketch en p5.js en tu bitácora.

    https://editor.p5js.org/isaacrisi/sketches/a7lKIWhhs 
    
  - Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

    <img width="606" height="396" alt="image" src="https://github.com/user-attachments/assets/056b40c9-4b71-498e-b82f-286408806652" />

    




  

    

  
