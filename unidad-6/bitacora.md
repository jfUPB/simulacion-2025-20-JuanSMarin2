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
  
### Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el movimiento y comportamiento colectivo de los agentes

Utilizo una distribución gaussiana para generar el campo, se elige un centro y todos los vectores del campo tienden a apuntar hacia ese punto. De esta manera la mayoría de los agentes se agrupan en una misma posición. Al hacer clic, se elige un nuevo centro y los vectores se reajustan, cambiando también el movimiento y la organización de los agentes.

![Grabación-de-pantalla-2025-09-30-173436](https://github.com/user-attachments/assets/44868de3-6dc5-400b-832f-97aedf2a0005)

``` js
class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / this.resolution);
    this.rows = floor(height / this.resolution);
    this.field = Array.from({ length: this.cols }, () => new Array(this.rows));
    this.numSources = 6;           
    this.sigmaRange = [80, 220];     
    this.init();
  }


  init() {
    this.sources = [];
    for (let k = 0; k < this.numSources; k++) {
      this.sources.push({
        pos: createVector(random(width), random(height)),
        sigma: random(this.sigmaRange[0], this.sigmaRange[1]),
        sign: random([-1, 1]),          
        swirl: random() < 0.5 ? 0 : 1,    
        strength: random(0.8, 1.6)
      });
    }

    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        const w = this.resolution;
        const h = this.resolution;
        const x = i * w + w * 0.5;
        const y = j * h + h * 0.5;
        const p = createVector(x, y);

        let v = createVector(0, 0);

        for (const s of this.sources) {
          const d = p5.Vector.sub(s.pos, p);
          const r2 = d.magSq();
          const twoSigma2 = 2 * s.sigma * s.sigma;
          const weight = Math.exp(-r2 / twoSigma2) * s.strength;

          if (weight < 1e-4) continue;

          let contrib = d.copy();
          if (s.swirl === 1) {
   
            contrib = createVector(-d.y, d.x);
          }

          contrib.setMag(weight);
          contrib.mult(s.sign);
          v.add(contrib);
        }

        if (v.mag() < 1e-6) {

          v = p5.Vector.fromAngle(random(TWO_PI)).mult(0.001);
        } else {
          v.normalize();
        }
        this.field[i][j] = v;
      }
    }
  }

  show() {
    stroke(0, 60);
    strokeWeight(1);
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        const w = this.resolution;
        const h = this.resolution;
        const x = i * w + w * 0.5;
        const y = j * h + h * 0.5;
        const v = this.field[i][j].copy().mult(w * 0.5);
        line(x, y, x + v.x, y + v.y);
      }
    }
  }

  lookup(position) {
    const column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
    const row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

```

## Actividad 04
### Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
En el metodo Flock se calculan las 3 reglas atravez de sus propias funciones:
#### Separación
Primero detecta a los boids cercanos, en este caso a 25 unidades de distancia aunque el autor aclara que este valor es arbitrario y se puede cambiar.
Despues si el boid esta entre la distancia determinada y 0 calcula un vector para alejarse y este vector es el que la funcion devuelve y aplica como fuerza.
#### Alineación
Calcula la velocidad de todos los boids cercanos y lo alinea usando la formula de Reynolds’s (steer = desired – velocity)
#### Cohesión
Es muy parecido al de alineación pero en vez de calcular la velocidad de los boids cercanos, calcula la posicion y la convierte en el lugar deseado

### Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
* Radio de percepción: Para hacer los calculos de todos los metodos del flocking solo se tienen en cuenta a los agentes cercanos, este parametro es la distancia en la cual si hay un boid a esta distancia se toma en cuenta para el calculo.
* r: Radio para dibujar al boid.
* maxSpeed: Velocidad maxima que puede tener un boid.
* masForce: Fuerza maxima que se le puede aplicar a un boid.
* Pesos de las reglas: Multiplicador para el vector calculado en cada metodo de flocking.

  ### Describe la modificación que realizaste al código y explica detalladamente el efecto que tuvo en el comportamiento

Cambie el radio de percepcion de los metodos de flocking, puse en 0 la separación y la alineación y aunmente exageradamente la cohesión a 1000.
Esto causa un efecto interesante, al principio todos los boids se agrupan en un solo lugar como era de esperarse, el lugar en el que se agrupan se mueve lentamente. 
Lo interesante sucede cuando se generan nuevos boids y cuando llegan a una esquina, en vez de unirse al centro empiezan a orbitar el cumulo lo que a su vez afecta el movimiento de los boids que hacen parte del cumulo, quedan orbitando como si fueran los anillos de saturno.

![Grabación-de-pantalla-2025-09-30-181918](https://github.com/user-attachments/assets/3e7d97c9-8b06-49e0-a7db-22b5285a95a3)

``` ja

class Boid {
  constructor(x, y) {
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 1));
    this.position = createVector(x, y);
    this.r = 3.0;
    this.maxspeed = 3; // Maximum speed
    this.maxforce = 0.05; // Maximum steering force
  }

  run(boids) {
    this.flock(boids);
    this.update();
    this.borders();
    this.show();
  }

  applyForce(force) {
    // We could add mass here if we want A = F / M
    this.acceleration.add(force);
  }

  // We accumulate a new acceleration each time based on three rules
  flock(boids) {
    let sep = this.separate(boids); // Separation
    let ali = this.align(boids); // Alignment
    let coh = this.cohere(boids); // Cohesion
    // Arbitrarily weight these forces
    sep.mult(1.5);
    ali.mult(1.0);
    coh.mult(1.0);
    // Add the force vectors to acceleration
    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);
  }

  // Method to update location
  update() {
    // Update velocity
    this.velocity.add(this.acceleration);
    // Limit speed
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    // Reset accelertion to 0 each cycle
    this.acceleration.mult(0);
  }

  // A method that calculates and applies a steering force towards a target
  // STEER = DESIRED MINUS VELOCITY
  seek(target) {
    let desired = p5.Vector.sub(target, this.position); // A vector pointing from the location to the target
    // Normalize desired and scale to maximum speed
    desired.normalize();
    desired.mult(this.maxspeed);
    // Steering = Desired minus Velocity
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce); // Limit to maximum steering force
    return steer;
  }

  show() {
    // Draw a triangle rotated in the direction of velocity
    let angle = this.velocity.heading();
    fill(127);
    stroke(0);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    beginShape();
    vertex(this.r * 2, 0);
    vertex(-this.r * 2, -this.r);
    vertex(-this.r * 2, this.r);
    endShape(CLOSE);
    pop();
  }

  // Wraparound
  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  // Separation
  // Method checks for nearby boids and steers away
  separate(boids) {
    let desiredSeparation = 0;
    let steer = createVector(0, 0);
    let count = 0;
    // For every boid in the system, check if it's too close
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      // If the distance is greater than 0 and less than an arbitrary amount (0 when you are yourself)
      if (d > 0 && d < desiredSeparation) {
        // Calculate vector pointing away from neighbor
        let diff = p5.Vector.sub(this.position, boids[i].position);
        diff.normalize();
        diff.div(d); // Weight by distance
        steer.add(diff);
        count++; // Keep track of how many
      }
    }
    // Average -- divide by how many
    if (count > 0) {
      steer.div(count);
    }

    // As long as the vector is greater than 0
    if (steer.mag() > 0) {
      // Implement Reynolds: Steering = Desired - Velocity
      steer.normalize();
      steer.mult(this.maxspeed);
      steer.sub(this.velocity);
      steer.limit(this.maxforce);
    }
    return steer;
  }

  // Alignment
  // For every nearby boid in the system, calculate the average velocity
  align(boids) {
    let neighborDistance = 0;
    let sum = createVector(0, 0);
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.normalize();
      sum.mult(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector(0, 0);
    }
  }

  // Cohesion
  // For the average location (i.e. center) of all nearby boids, calculate steering vector towards that location
  cohere(boids) {
    let neighborDistance = 10000;
    let sum = createVector(0, 0); // Start with empty vector to accumulate all locations
    let count = 0;
    for (let i = 0; i < boids.length; i++) {
      let d = p5.Vector.dist(this.position, boids[i].position);
      if (d > 0 && d < neighborDistance) {
        sum.add(boids[i].position); // Add location
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum); // Steer towards the location
    } else {
      return createVector(0, 0);
    }
  }
}
```



# Apply: Aplicación
## ¿Qué quiero hacer? – Inspiraciones
Quiero seguir complementando la simulación que empecé en la unidad pasada. Cuando veo los boids del ejemplo de flocking siempre me parece que son renacuajos nadando en la laguna por lo que quiero aprovechar esto para hacer la simulación. Tomando de inspiración mi propia obra con las mismas inspiraciones.


Pensando en como aplicar musica sin repetir concepto y haciendo algo que altere los movimientos de los renacuajos se me ocurre hacer algo similar a la simulación de Esteban de la unidad pasada, cada nota será un pedaso de comida al que se convertira en la ubicación deseada de los renacuajos.
<img width="719" height="500" alt="image" src="https://github.com/user-attachments/assets/a59da2f5-2eb0-4fdd-bdc2-0cda90a2303f" />

La cancion que se me ocurre es "When Mother Was There" que tambien es de Persona 5, la escojo porque necesito una cancion lenta pero con mucha variedad de instrumentos ya que alterando los parametros del flocking puedo hacer que el movimiento de los renacuajos varie mucho.

La melodia principal de la cancion se dispone de dos momentos, una serie de notas que se representa como una pregunta y otra que es como la respuesta. Quiero que la primera se represente con la caida de un alimento y la respuesta sea el movimiento de los boids a este.
<img width="717" height="497" alt="image" src="https://github.com/user-attachments/assets/98580d2c-4a34-4d83-95c9-d70c09bc7c3d" />
<img width="848" height="590" alt="image" src="https://github.com/user-attachments/assets/6c458c63-b8e3-4572-a1c2-4d5fd686ac28" />

Igual que la simulación anterior, el alimento caera como vista top down y al impactar con el agua dejara una honda pero en esta no desaparece solo al caer si no que desaparece un segundo despues de que un agente lo toque para que no desaparezca muy rapido y para que los renacuajos alcancen a dirigirse a este.

Pero la canción no solo se compone de melodia. Hay una parte en el que la melodia principal se detiene y se cambia por una melodia diferente de guitarra. Pero este se sigue comportando como pregunta y respuesta aunque dejando mas tiempo vacio entre estos. Estas se representaran con cambios en el radio de detección de los 3 metodos del flocking. Escojo representarlos con esta parte de la cancion ya que se necesita mas tiempo para apreciar los cambios que se generar al alterar la separación, alineación y cohesión por lo que los cambios de secciones de la cancion mas demorados de esta parte de la cancion cuadran perfecto para esto.
<img width="970" height="674" alt="image" src="https://github.com/user-attachments/assets/f85ee02c-a7be-482f-a4a4-a0acd719d118" />
<img width="711" height="494" alt="image" src="https://github.com/user-attachments/assets/9d5ebc06-f4b2-41dd-b875-6d75719a698c" />
<img width="846" height="587" alt="image" src="https://github.com/user-attachments/assets/69a90d4a-9d03-4723-89a9-1804a93c9e25" />
<img width="850" height="591" alt="image" src="https://github.com/user-attachments/assets/4a9580b3-90c2-4879-aa05-1de1e05a2df5" />

El paso de esta parte de la canción devuelta a la primera tiene como una explosión lo que es perfecto ya que el cambio de separacion y alineacion de 0 a 50 hace que los renacuajos se dispercen como una explosión por lo que este sera el que lo represente.

La percución se representara con las mismas gotas triangulares y hondas de decagono de la unidad anterior que dejan una honda pero en este caso tambien cambian el color del lago entre tonos de azul

<img width="593" height="415" alt="image" src="https://github.com/user-attachments/assets/995ac50a-6e9e-4dd8-a9ec-5ec74fb10b7b" />

<img width="585" height="404" alt="image" src="https://github.com/user-attachments/assets/1fcbe823-1a7f-4c62-b318-aed55e0cbc03" />


