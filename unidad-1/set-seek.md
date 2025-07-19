# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01: La aleatoriedad en el arte generativo
#### Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo.
Permite variabilidad haciendo que la pieza de arte sea diferente cada vez que se ejecuta.

### Actividad 02: Ejemplo de aleatoriedad en el arte generativo
#### Cuál es el papel de la aleatoriedad en su obra.
Se encarga de potenciar la metafora de la naturaleza aleatoria de la vida, no hay patron ni onda en la obra que se genere exactamente igual asi como en la realidad no hay dos arboles que crezcan iguales.
#### Según tu perfil profesional, cómo se aplica el concepto de aleatoriedad en el tipo de proyectos que desarrollas. Ilustra tu respuesta con ejemplos concretos.
En el desarrollo de videojuegos la aletoriedad la aletoriedad sirve para generar experiencias diferentes, podria usarla que un juego finito se vuelva infinito como con generación de un terreno procedural o generando emoción o tensión en el jugador dependiendo de su suerte como al hacer un golpe critico en RPG. Sin aletoriedad cada partida seria linear y exactamente igual si se toman las mismas desiciones, con la aletoriedad cada partida puede 
ser diferente y emocionante.
### Actividad 03: Caminatas aleatorias
#### Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.
Voy a cambiar las probibilidades para que el movimiento del punto deje de ser uniforme y se vuelva no uniforme, voy a aumentar las probabilidades del movimiento en x
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

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
    this.x = width / 2;
    this.y = height / 2;

  }

  show() {
    stroke(mouseX, mouseY, random(256));
    
    strokeWeight(5)
    square(this.x, this.y, 10);
    
  }

  step() {
    const choice = floor(random(8));
    if (choice == 0 || choice == 5 || choice == 6) {
      this.x++;
    } else if (choice == 1 || choice == 3 || choice == 4) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}



```

#### Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
Espero que se pinte en forma de cuadrado, se quede en el centro y no varie mucho en ninguna de las direcciones pero su posicion horizontal cambia mas que la vertical, tambien espero que el cuadrado se vuelva mas grande y cambia de color en base a la posicion del mouse debido a stroke(mouseX, mouseY, random(256)); y strokeWeight(5)

#### Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
<img width="494" height="264" alt="image" src="https://github.com/user-attachments/assets/481d76ad-f8b9-4c2f-9294-aa57ebf0f33c" />

<img width="379" height="205" alt="image" src="https://github.com/user-attachments/assets/095648bd-3d8f-437f-a33a-2c3cc89cb856" /> //Experimento con 2 walkers

Los cuadros cambian de tono aleatoriamente y su color cambia en base a la posicion del mouse, el movimiento de los cuadros se mantiene en el centro y cambian de posicion mas en X que en Y

#### Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
Si ocurrió lo que esperaba en cuanto al movimiento del cuadrado ya que tienen mas probabilidad de moverse en X que en Y y se mantienen en el centro ya que la probabilidad de aumentar y disminuir es igual tanto en X como en Y

### Actividad 04: Distribuciones de probabilidad
#### En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
Una distribución uniforme es esa en la que cualquier valor posible tiene las mismas posibilidades de suceder como al tirar un dado.
#####
En una distribución no uniforme no todos lo valores tienen la misma probabilidad como en la distribución gaussiana en la que es más probable que salga un valor cercano a la media.
#### Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.
Afortunadamente el experimento que hice fué muy similar a esto, solo hay que hacer que aumentar en X suceda con 3 valores posibles y dejando el resto en solo 1.

```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

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
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(6));
    if (choice == 0 || choice == 3 || choice ==4) { // No tienen que ser 3 necesariamente, puede ser 2 o mas
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```
<img width="364" height="180" alt="image" src="https://github.com/user-attachments/assets/a25f78e2-65b5-4fc5-80db-f4d85c393ec0" />

### Actividad 05: Distribución Normal
#### Una vez has entendido el concepto de distribución normal, vas a pensar en una nueva manera de visualizarlo.
```
function setup() {
  createCanvas(400, 400);
    background(220);
}

function draw() {

       var mean = 10;
      var std = 30;
  circle(200,200 ,randomGaussian(mean,std) )
  
  stroke(mouseX,mouseY,random(255))
}
```
A medida que el programa corre, la mayoría de los circulos tienen tamaños similares ya que los valores aleatorios se concentran cerca de la media. Solo en raras ocasiones aparecen círculos más grandes o más pequeños, lo que no ocurre con una distribución uniforme. 
#####
El perimetro de los circulos tambien cambia de color en base a la posición del mouse pero esto no tiene nada que ver con la distribución normal.
##### Enlace: https://editor.p5js.org/JuanSMarin2/sketches/BQWxw2T0d
<img width="332" height="344" alt="image" src="https://github.com/user-attachments/assets/9cb625f3-db6c-4d4f-ba0a-5a959a6b9e3c" />
<img width="236" height="256" alt="image" src="https://github.com/user-attachments/assets/16c002d3-83b6-4afb-bd58-2966dc48fd11" />


### Actividad 06: Distribución personalizada: Lévy flight
#### Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.
Modifiqué el ejercicio de la actividad 03
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

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
    this.x = width / 2;
    this.y = height / 2;

  }

  show() {
    stroke(mouseX, mouseY, random(256));
    
    strokeWeight(5)
    square(this.x, this.y, 10);
    
  }

  step() {
    const choice = floor(random(8));
    const jump = random(1);
    var amount;
    
    if(jump > 0.01){
      amount = 1;
    } 
    else
    {
      amount = 50;
    }
    
    if (choice == 0 || choice == 5 || choice == 6) {
            this.x += amount;
  
    } else if (choice == 1 || choice == 3 || choice == 4) {
      this.x -= amount;
      
    } else if (choice == 2) {
      this.y += amount;
    } else {
      this.y -= amount;
    }
  }
}


```
#### Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
Espero que los cuadrados se muevan igual que antes pero cada cierto tiempo saltan a otra posición cercana.

#### Ejecuta el código y escribe en tu bitácora qué sucedió realmente.

<img width="311" height="218" alt="image" src="https://github.com/user-attachments/assets/f5fd6cbd-d5ef-4604-84a8-28df79ece686" />
Debido a que el programa original no variaba mucho de moviemiento y se quedaba mucho en un punto, despues de un tiempo quedan grupos de cuadros que no suelen chocar entre si, tambien tiene mayor probabilidad de hacer un salto en X que en Y

#### Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
De cierta manera sí, pero esperaba que fuera más variado y no terminara generando grupos separados de elementos.
<img width="379" height="238" alt="image" src="https://github.com/user-attachments/assets/770d1f91-2191-4f56-943c-c7a126c03950" />
##### Enlace: https://editor.p5js.org/JuanSMarin2/sketches/EQX1YkSVW
### Actividad 07: Ruido Perlin
#### Una vez has entendido el concepto de ruido Perlin, vas a pensar en una nueva manera de visualizarlo.

```
let tx;
let ty;

function setup() {
  createCanvas(500, 500);
    background(200);
  tx = 0;
  ty = 1000; 
}

function draw() {


 
  let x = noise(tx) * width;
  let y = noise(ty) * height;

  strokeWeight(5);
  point(x, y);

  tx += 0.01;
  ty += 0.01;
}
function mouseClicked() {
   background(200);
}

```

#### Explica el concepto qué resultados esberabas obtener. 
Hice algo similar al ejemplo de la actividad 2 pero espero que gracias al metodo noise la trayectoria sea mucho más limpia, tambien hice que al hacer clic el trazo se borre
<img width="469" height="447" alt="image" src="https://github.com/user-attachments/assets/2cc9eee5-bc7e-4d1e-b33d-16cf3b2a5b8a" />
El movimiento sigue siendo aleatorio pero ya es mucho menos herratico, ya se ve mas ordenado y redondeado, parece un trazo de lapiz en vez de puntos aleatorios
##### Enlace: https://editor.p5js.org/JuanSMarin2/sketches/bD7GX_TgB

