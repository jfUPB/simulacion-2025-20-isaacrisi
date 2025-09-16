# Evidencias de la unidad 5

## Actividad 2

1er ejemplo 

En este caso cada partícula es creada, almacenada y actualizada hasta que su vida (lifespan) llega a 0. En ese momento, se elimina del arreglo. Esto evita que el sistema se sature, manteniendo la memoria bajo control.

El concepto aplicado en esta modificacion fue un levy flight en lugar de la aparcicion en random, lo que hace que algunas particulas aprezcan normalmente y otras volando.

En este sistema ya hay un control de particulas como dije en un inicio.

- Implementé Levy Flights usando una función levyFlightStep() basada en una distribución de potencia inversa.

- Sustituí la asignación aleatoria simple de la velocidad inicial por un vector generado por el flight.

- Verifiqué que las partículas ahora realicen movimientos largos esporádicos y erráticos, típicos del comportamiento Levy.

https://editor.p5js.org/isaacrisi/sketches/X19PpS1Bj

https://editor.p5js.org/natureofcode/sketches/-xTbGZMim

<img width="743" height="334" alt="image" src="https://github.com/user-attachments/assets/aeef6eab-c3dd-4370-94a6-021f474850cd" />

2do ejemplo 

En este caso el sistema de particulas se maneja de la misma forma que el anterior on el metodo del lifespan y el splice.

El concepto aplicado en esta modificacion fue el ruido de perlin para ver como la diferencia que se marcaba con el levy flight, viendo que ese componente agresivo se desaparece y se ve algo como mas armonico 

La variacion se hizo de la misma forma pero con un noise() en ligar de levyFlightStep().

https://editor.p5js.org/isaacrisi/sketches/v8BnzQ_EA

https://editor.p5js.org/natureofcode/sketches/2ZlNJp2EW

<img width="666" height="256" alt="image" src="https://github.com/user-attachments/assets/69ce5544-f56e-4fab-8da2-d12c18860419" />

3er ejemplo

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


4to ejemplo 

Creación: Cada frame se añade una nueva partícula en la posición del emisor con addParticle().

Desaparición: Cada partícula tiene un lifespan que disminuye con el tiempo. Cuando es menor que 0, se elimina del array usando splice().

Memoria: Al eliminar partículas muertas, se libera memoria evitando que la lista crezca indefinidamente, manteniendo buen rendimiento.

Se aplica el concepto de motion 101 con aceleracion incluida

La aceleración, que antes era constante (gravedad), ahora es una fuerza aleatoria que cambia cada frame.

Esto genera un movimiento más orgánico e impredecible en las partículas.

https://editor.p5js.org/isaacrisi/sketches/hs5cSGpkx

https://editor.p5js.org/natureofcode/sketches/uZ9CfjLHL

<img width="614" height="418" alt="image" src="https://github.com/user-attachments/assets/cae5692f-bd3b-40db-be46-844a20720e6e" />


 




