# Evidencias de la unidad 7

# Actividad 1
## Tu análisis de 3-4 ejemplos de Ji Lee, explicando cómo logran la conexión palabra-imagen.

Noté que hay algunas conexiones que utilizan la forma de las letras y no solo el comportamiento de la palabra y siento que estas son las mas creativas e interesantes
### Vampiro:
Al girar la palabra la M parece un par de colmillos como los que tiene un vampiro y la animación los muestra chupando la sangre de alguien.

### Balloon:
Las dos O de la palabra se elevan como si fueran globos, muy simple pero creativa.

### Tunnel
Esta fue la que mas me gustó, las dos n de la palabra representan la entrada y salida del tunel, es demasiado creativo, nunca se me hubiera ocurrido.

## Tus propias ideas (descripción o boceto simple) para representar visualmente 2-3 palabras distintas de forma estática.
### Seek:
Se me ocurrió mirando la bitacora, me di cuenta que las Words As Image que mas me gustaron eran los que tenian una letra repetida. 
Para este me imagino que la primera e es un investigador que utiliza la otra e como una lupa 
<img width="343" height="167" alt="image" src="https://github.com/user-attachments/assets/498e7bce-b919-44ab-91eb-b1d926e03e0d" />

(esto es muy dificil)

### Battle
Las dos t estan en un duelo. La de la derecha ataca con su espada que es la l y la de la izquierda se defiende con su escudo que es la a


<img width="446" height="196" alt="image" src="https://github.com/user-attachments/assets/48dac135-0b64-4bef-93cf-243b5d79fe39" />

<img width="464" height="241" alt="image" src="https://github.com/user-attachments/assets/518dfc8e-31cd-4af2-9468-b18273c13ee7" />


# Actividad 2

### Experimento 1: Dos cajas y una superficie
``` js

const {Engine, Body, Bodies, Composite, Render, Runner} = Matter;

let engine
let render

function setup() {
  
  noCanvas();
  engine = Engine.create();
  
  render = Render.create({
    element: document.body,
    engine: engine,
    
    
});
  
  
  var boxA = Bodies.rectangle(400, 200, 80, 80);
var boxB = Bodies.rectangle(450, 50, 80, 80);
var ground = Bodies.rectangle(400, 610, 810, 60, { isStatic: true });
  
  Composite.add(engine.world, [boxA, boxB, ground]);
  
  Render.run(render);
  
  var runner = Runner.create();


Runner.run(runner, engine);
}


```
### Link: https://editor.p5js.org/JuanSMarin2/sketches/QK8ZvURWG
<img width="231" height="296" alt="image" src="https://github.com/user-attachments/assets/77580a01-f061-41c0-b014-88907881df7a" />


### Experimento 2: Con p5.js

``` js
const { Engine, Body, Bodies, Composite } = Matter;

let engine, ground, boxes = [];

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();
  ground = new Box(200, 300, 400, 10, true);
}

function draw() {
  background(220);
  Engine.update(engine);
  boxes.forEach(b => b.display());
  ground.display();
}

function mousePressed() {
  boxes.push(new Box(mouseX, mouseY, 20, 20));
}

class Box {
  constructor(x, y, w, h, isStatic = false) {
    this.w = w;
    this.h = h;
    this.body = Bodies.rectangle(x, y, w, h, { isStatic });
    if (!isStatic) Body.setAngularVelocity(this.body, 0.2);
    Composite.add(engine.world, this.body);
  }

  display() {
    const { x, y } = this.body.position;
    const { angle } = this.body;
    push();
    translate(x, y);
    rotate(angle);
    rectMode(CENTER);
    rect(0, 0, this.w, this.h);
    pop();
  }
}

```
### Link: https://editor.p5js.org/JuanSMarin2/sketches/dAiOspLZR
<img width="398" height="396" alt="image" src="https://github.com/user-attachments/assets/84c7ebac-151b-427f-9e09-2457bfcf99d1" />
<img width="400" height="394" alt="image" src="https://github.com/user-attachments/assets/b8c79792-c32d-4b83-9933-9cc07bd35601" />

## Proporciona tu explicación clara y concisa de los conceptos clave (Engine, World, Bodies, Constraint, MouseConstraint).
### Engine 
Es el programa que maneja la simulación de fisicas, es algo asi como unity o el propio p5.

### World
Es el contenedor de todos los objetos y de todo lo que hay en la simulación, si un cuerpo no esta en el mundo las fisicas no se aplicaran en este.

### Body
Es un objeto individual en la simulación, puede ser estatico como el suelo y dinamico como una caja que cae.

### Bodies
Es un contenedor que agrupa tipos especificos de cuerpos con propiedades definidas, como un prefab en Unity.

### Constrain
Una restricción o unión entre dos cuerpos, como una cuerda

### MouseConstraint
Permite la interacción permitiendo mover cuerpos con el mouse

## Menciona brevemente cualquier dificultad encontrada al configurar o usar Matter.js inicialmente.
Fue pasar de la teoria a lo practico, entender como se usan los conceptos y como funcionan realmente en codigo, ChatGPT se me pusó loco por lo que me tocó meterme a la documentación para entender los conceptos y tambien seguí el video para hacer funcionar las simulación.



# Apply: Aplicación 🛠
## Indica claramente la palabra elegida. 
Battle 

## Explica tu idea conceptual: ¿Cómo la animación física representa el significado de la palabra?
Viendo los ejemplos del video Words as an Image, me di cuenta de que las animaciones que más me gustaron no solo representaban el comportamiento de la palabra, sino que también aprovechaban las letras para que parecieran lo que la palabra significa.
Además, noté que las que mejor lograban esto eran las que tenían una letra repetida.
Por eso escogí la palabra Battle, que significa batalla: las dos t están en un duelo.
La t de la derecha ataca con su espada (la L) y la t de la izquierda se defiende con su escudo (la a).

## Describe brevemente los aspectos técnicos clave de tu implementación: ¿Cómo formaste las letras con Matter.js? ¿Qué propiedades físicas fueron importantes? ¿Usaste restricciones?
Utilicé un Constraint para pegar las armas a sus respectivas t.
<img width="774" height="534" alt="image" src="https://github.com/user-attachments/assets/97b401e2-eca4-41fb-9751-e9eae0a3d405" />

Con un click se puede iniciar el choque: cuando se hace clic, la a y la L empiezan a atraerse para generar el efecto de que están chocando las espadas.
En el momento en que colisionan, aparece una partícula de colisión, que es una estrella amarilla que crece rápidamente y después se desvanece.

<img width="913" height="629" alt="image" src="https://github.com/user-attachments/assets/d8ae8346-ccf0-440f-89b3-9093aaceced9" />
<img width="1086" height="746" alt="image" src="https://github.com/user-attachments/assets/fa0dd86c-d7d6-4fd2-947b-036cdbb32c99" />


Cuando impactan, las letras se repelen y luego vuelven a su estado natural.
El movimiento entre los constraints pasa suavemente de la parte izquierda de la t a la derecha para atacar, y viceversa para volver al estado base.
<img width="926" height="313" alt="image" src="https://github.com/user-attachments/assets/be6ae931-8114-4c20-b828-d6ea6c85dbe3" />


