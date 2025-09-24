# Evidencias de la unidad 6


## Actividad 01

### Captura en tu bitácora dos imágenes de Tyler Hobbs que te llamen la atención y explica por qué.

<img width="742" height="749" alt="image" src="https://github.com/user-attachments/assets/3436dfd7-4d23-4583-ad92-1f0f83e60dc2" />



Me gusta como esta obra crea una figura que se asemeja tanto a un arbol y a unas raices a pesar de ser lineas creadas unicamente con angulos aleatorios por vector.

<img width="680" height="668" alt="image" src="https://github.com/user-attachments/assets/3abe818a-8997-4358-a6ec-2fb346cab0b6" />


Me llama la atención que en esta obra no hay ninguna foto, solo hay figuras con colores pero a pesar de los espacios vacios entre ellas se crea la ilusión de que hay una fotografia.


### ¿Qué te inspira de su trabajo?
Como él es capaz de generar imagenes tan distintas simplemente cambiando parametros. Como experimenta con sus obras para ver que es lo que sucede y como quedan obras tan variadas, asi como las de las ramas, no creo que hubiera sido intencional y aun asi logró asemejarse a un fenomeno real. Me inspira a experimentar de misma manera con mis obras para ver que cambios tan variados puedo generar.

## Actividad 02
### ¿Qué es una fuerza de dirección (steering force)?
Es una fuerza para corregir el movimiento de un objeto, hace que un objeto se mueva al lugar se desea corrigiendo el vector de velocidad.

### ¿Qué diferencia tiene este tipo de fuerza con las que ya hemos estudiado en el contexto de la simulación de agentes?
Las fuerzas que hemos estudiado afectan el movimiento y la rapidez ya sea aumentandola o disminuyendola como la fricción, hasta ahora no teniamos ninguna fuerza (Aparte de la gravedad) que influyera en la dirección en la que se mueve el objeto haciendo que tienda a una posición exacta. 

### ¿Qué relación tiene la steering force con Craig Reynolds y su trabajo en simulación de comportamiento animal?
Con esta se pueden hacer movimientos mas naturales para simular el comportamiento animal, un animal no se mueve con ruido perlin o saltos de levy. Se pueden hacer movimientos que parezcan desiciones como escapar o ir a un sitio especifico.


## Actividad 03

### Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
El flujo se genera como un Array de que almacena un dos vectores, de columnas y de filas. 
Los vectores se obtienen dividiendo el alto y ancho del canvas con la resolución de la pantalla. La resolución es un parametro que se escoge en el constructor.
``` js
 flowfield = new FlowField(20);

constructor(r) {
    this.resolution = r;

    this.cols = width / this.resolution;
    this.rows = height / this.resolution;

    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }
    this.init();
  }

```

### Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
El agente le da su posicion al campo de flujo para que este le de el parametro de la ubicación deseada, despues evitando que se pase de la velocidad maxima, se aplica la fuerza de corrección calculada restando el deseo con la velocidad actual 

``` js
 follow(flow) {

    let desired = flow.lookup(this.position);

    desired.mult(this.maxspeed);
 
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

```



### Lista los parámetros clave identificados (resolución, maxspeed, maxforce).

* La resolución: Se pasa como parametro en el constructor del campo de flujo y se usa para calcular el tamaño y numero de las filas y columnas
* Motion 101: Siguen estando los parametros clave para hacer Motion 101, la posición, velocidad y aceleración. siguen haciendo lo de siempre, cada una altera al vector anterior para generar movimiento.
* maxSpeed: Controla la velocidad maxima a la que el agente puede viajar.
* maxForce: Limita el cambio de fuerza al que se le puede aplicar al objeto para no hacer cambios muy abruptos
  



