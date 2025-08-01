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

### Actividad 04: Explora posibilidades
#### ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
Mag sirve para calcular la magnitud de un vector. MagSq calcula la magnitud del vector al cuadrado, esta ultima es mas eficiente ya que da no hay que lidear con la raiz cuadrada
#### ¿Para qué sirve el método normalize()?
Sirve para convertir la magnitud de un vector a 1 volviendolo unitario pero conservando su dirección, puede usarse para aplicar una fuerza constante sin importar la distancia original
#### Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
Calcula el producto punto entre dos vectores, sirve para saber qué tanto dos direcciones apuntan hacia la misma dirección.
#### Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.
El producto cruz da un nuevo vector perpendicular a los dos vectores originales, su dirección depende del orden de los vectores y su tamaño depende del de la superficie entre ellos.
#### Para que te puede servir el método dist()?
Sirve para calcular la distancia entre dos puntos

#### ¿Para qué sirven los métodos normalize() y limit()?
Normalize ya lo mencione previamente, limit limita (valga la redundancia) la magnitud posible de un vector para que no pueda superarlo


## Duda extra que me surgio

#### ¿Qué pasa con los componentes de los vectores al aplicar los métodos normalize y limit?
Con normalize cada componente se divide por la magnitud lo que hace que al sumarse den igual a 1.

Por ejemplo
``` js 
let v = createVector(3, 4);

```
Este vector tiene magnitud: 5 por lo que 


<img width="494" height="109" alt="image" src="https://github.com/user-attachments/assets/05a02d32-4321-43b8-bb53-9359b93c4481" />


y v.mag ahora devuelve exactamente 1

Limit hace esta misma operación primero y lo multiplica por el limite para que la magnitud de igual al limite

Por ejemplo
``` js
et v = createVector(6, 8);
v.limit(5);
```
Como 10 > 5 primero se normaliza 


<img width="306" height="66" alt="image" src="https://github.com/user-attachments/assets/677fa429-c4ad-4391-8456-462d71c531b5" />


Y luego se multiplica por 5 (El limite)


<img width="354" height="46" alt="image" src="https://github.com/user-attachments/assets/2563d098-d154-498f-9831-e49abdbb176d" />


Entonces el vector final es (3, 4) con magnitud 5.


### Actividad 05: Interpolamos?
#### El código que genera el resultado que te pedí.
``` js
let movementCalculator;
let amount;

function setup() {
    createCanvas(400, 400);
  movementCalculator = 0.5;
  amount = 0.01;
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(200, 0);
    let v2 = createVector(0, 200);
    let v3 = p5.Vector.lerp(v1, v2, movementCalculator);
    let v4 = createVector(250,50);
    let v5 = createVector(50,250);
    let v6 = p5.Vector.sub(v5,v4);
  
  let fromColor = 'red';
  let toColor = 'blue';
  let v6Color = lerpColor(fromColor,toColor, movementCalculator);
  
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, v6Color);
    drawArrow(v4, v6, 'green');
  
  
  
  if(movementCalculator >= 1 && amount > 0){
    amount *= -1;
    
  }
  else if(movementCalculator <= 0 && amount < 0){
    amount *= -1;
  }
      movementCalculator += amount;
   
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}

```
Creo que la parte para calcular la dirección del amount se puede hacer en una sola linea pero asi funciona

#### Link: https://editor.p5js.org/JuanSMarin2/sketches/uINYenqFp
<img width="398" height="400" alt="image" src="https://github.com/user-attachments/assets/9b0c9415-cc76-46d8-8fa1-a83f567d3761" />
<img width="424" height="414" alt="image" src="https://github.com/user-attachments/assets/78e23a07-7591-49bf-b381-c00b0dcb6291" />

#### ¿Cómo funciona lerp() y lerpColor().
Lerp sirve para interpolar un vector en base a dos vectores, es decir, se mueve en la linea que forman los dos vectores en base al tercer parametro.

LerpColor hace lo mismo pero entre dos colores

#### ¿Cómo se dibuja una flecha usando drawArrow()?
En base al origen de coordenadar dado por el primer vector, el triangulo se crea 7 unidades antes de la punta de la linea y ubica las lineas del triangulo desde los puntos 1. (0, 7/2) 2. (0,-7/2) y 3.(7,0)


### Actividad 06: Motion 101
#### Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.
Es lo que se hacia en la actividad 02. Al vector de posición se le suma el vector de velocidad en cada frame para obtener una nueva dirección. Tambien se le puede sumar la aceleración a la velocidad para que esta varie.
#### ¿Cómo se aplica motion 101 en el ejemplo?
Se esta usando en una clase Mover para aplicar el movimiento en una sola linea llamando al metodo update para no tener que aplicar draw y setup y para poder reutilizarlo facilmente.

### Actividad 07: Experimentando con la aceleración
### Para investigador el significado de esta frase te propone que construyas un experimento donde analices cómo se comporta un objeto en movimiento con:

#### Aceleración constante. 

La velocidad aumenta constantemente llendo cada vez más rápido hasta que casi no alcanza a verse o llega al limite

### Aceleración aleatoria

La velocidad aumenta y disminuye de manera aleatoria, toma mucha velocidad en un punto y despues disminuye.

### Aceleración hacia el mouse.
La bola se mueve hacia donde se encuentra el mouse, cuando este apunto de chocarse con el mouse empieza a orbitarlo.
Se hace encontrando la dirección al punto del mouse restando la posición del mouse con la posición de la bola.
