# Unidad 2

## 🔎 Fase: Set + Seek
### Actividad 01: Introducción a los vectores
#### 1. ¿Cómo funciona la suma dos vectores en p5.js?
Se puede hacer con el metodo add o con cada componente por ejemplo 
```  js
vector1.add(vector2); // Forma 1
vector1.x += vector2.x; // Forma 2
vector1.y += vector2.y;
```
#### 2. ¿Por qué esta línea position = position + velocity; no funciona?
Porque position y velocity no son vectores en p5.js, son variables que apuntan a los vectores en la memoria.

### Actividad 02: Repasa
#### ¿Qué tuviste que hacer para hacer la conversión propuesta?
Cree dos vectores: Velocidad y posición, en el metodo step dependiendo del numero aleatorio se varia la velocidad en una de las 4 direcciones. Tambien hice que al chocar con una esquina se invierte su velocidad, de esta forma se genera un trazo de puntos aleatorio 
``` js


let walker;



function setup() {
  createCanvas(640, 240);

  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
  this.position = createVector(width / 2,height / 2);
  this.velocity = createVector(2.5,2);
    
  }

  show() {
    stroke(0);
  this.position.add(this.velocity);
      if (this.position.x > width || this.position.x < 0) {
    this.velocity.x = this.velocity.x * -1;
  }
  if (this.position.y > height || this.position.y < 0) {
    this.velocity.y = this.velocity.y * -1;
  }

    point(this.position);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.velocity.x++;
    } else if (choice == 1) {
      this.velocity.x--;
    } else if (choice == 2) {
      this.velocity.y++;
    } else {
      this.velocity.y--;
    }
  }
}


```
<img width="641" height="242" alt="image" src="https://github.com/user-attachments/assets/a8b2f3a3-81ab-4552-b2bf-e389347977a4" />
#####
No tiene que hacerse con velocity pero quise experimentar, no salio un resultado fiel al walker original pero salio algo interesante

### Actividad 03: Experimenta
#### Dale una mirada a este código: 
``` js
let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9);
    console.log(position.toString());
    playingVector(position);
    console.log(position.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```
#### ¿Qué resultado esperas obtener en el programa anterior?
Espero que el programa de devuelva primero en consola la posición del vector que sera 6,9, despues mostrara 20,30 y saldra el mensaje only once
#### ¿Qué resultado obtuviste? 

<img width="370" height="116" alt="image" src="https://github.com/user-attachments/assets/c9dc3a88-7a5f-43f1-b724-e1952dd8dc74" />
#####
El log muestra un vector con 3 o mas parametros por eso el tercer parametro se muestra en 0
#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
##### Ejemplo paso por valor 
``` js
function setup() {
  createCanvas(400, 400);
  let x = 5;
  experiment(x);
  console.log(x); 
  
  
  
  noLoop();
}

function experiment(v) {
 v = 10000000;
}
```
Devuelve 5 porque no se modifico x en si
#### Ejemplo paso por referencia
``` js
let position;

function setup() {
    createCanvas(400, 400);
    position = createVector(6,9);
    console.log(position.toString());
    playingVector(position);
    console.log(position.toString());
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```
Hice trampa y puse el programa del ejemplo
#####
En este ejemplo se pasa el puntero al vector no el vector en si, los cambios si quedan porque se esta usando la referencia para modificar el propio vector en el metodo.

#### ¿Qué aprendiste?
Hay que tener cuidado viendo que se esta modificando realmente, hay que saber como funciona el motor o libreria que se esta usando para saber de que tipo de paso hay que usar para obtener lo que se desea.

