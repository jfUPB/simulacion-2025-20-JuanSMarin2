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
´´´
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



´´´

#### Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.
Espero que se pinte en forma de cuadrado, se quede en el centro y no varie mucho en ninguna de las direcciones pero su posicion horizontal cambia mas que la vertical, tambien espero que el cuadrado se vuelva mas grande y cambia de color en base a la posicion del mouse debido a stroke(mouseX, mouseY, random(256)); y strokeWeight(5)

#### Ejecuta el código y escribe en tu bitácora qué sucedió realmente.
<img width="494" height="264" alt="image" src="https://github.com/user-attachments/assets/481d76ad-f8b9-4c2f-9294-aa57ebf0f33c" />

<img width="379" height="205" alt="image" src="https://github.com/user-attachments/assets/095648bd-3d8f-437f-a33a-2c3cc89cb856" /> //Experimento con 2 walkers

Los cuadros cambian de tono aleatoriamente y su color cambia en base a la posicion del mouse, el movimiento de los cuadros se mantiene en el centro y cambian de posicion mas en X que en Y

#### Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?
Si ocurrió lo que esperaba en cuanto al movimiento del cuadrado ya que tienen mas probabilidad de moverse en X que en Y y se mantienen en el centro ya que la probabilidad de aumentar y disminuir es igual tanto en X como en Y





