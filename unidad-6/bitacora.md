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

´´´js 

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



