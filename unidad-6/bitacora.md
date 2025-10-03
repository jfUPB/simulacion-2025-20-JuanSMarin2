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

Quiero seguir complementando la simulación que inicié en la unidad pasada. Cada vez que observo los boids del ejemplo de flocking siempre me parecen renacuajos nadando en una laguna, por lo que quiero aprovechar esa idea para construir mi simulación. Me inspiro en mi propia obra y en las mismas referencias que ya he venido trabajando.

Pensando en cómo aplicar música sin repetir conceptos, y buscando una forma de alterar los movimientos de los renacuajos, se me ocurrió algo similar a la simulación de Esteban en la unidad pasada, pedazos de comida que se convertirán en la ubicación objetivo de los renacuajos.

<img width="719" height="500" alt="image" src="https://github.com/user-attachments/assets/a59da2f5-2eb0-4fdd-bdc2-0cda90a2303f" />

La canción que escogí es When Mother Was There, también de Persona 5. La elijo porque necesito una pieza lenta pero con mucha variedad instrumental, ya que al alterar los parámetros del flocking puedo generar variaciones interesantes en el movimiento de los renacuajos.

La melodía principal de esta canción se organiza en dos momentos: una serie de notas que suenan como una pregunta, y otra que funciona como la respuesta. Quiero representarlo de esta forma: la pregunta será la caída de un alimento, y la respuesta, el movimiento de los boids hacia él.

<img width="717" height="497" alt="image" src="https://github.com/user-attachments/assets/98580d2c-4a34-4d83-95c9-d70c09bc7c3d" /> <img width="848" height="590" alt="image" src="https://github.com/user-attachments/assets/6c458c63-b8e3-4572-a1c2-4d5fd686ac28" />

Al igual que en la simulación anterior, el alimento caerá con vista top-down, y al impactar con el agua generará una onda. Sin embargo, en este caso no desaparecerá solo al caer, sino un segundo después de que un agente lo toque. Esto permite que los renacuajos tengan suficiente tiempo para dirigirse hacia él.

La canción no se compone únicamente de la melodía principal. En cierto punto, esta se detiene y da paso a una melodía diferente de guitarra. Aunque mantiene la lógica de pregunta y respuesta, introduce más tiempo vacío entre cada frase. Esta sección se representará mediante cambios en el radio de detección de los tres comportamientos del flocking (separación, alineación y cohesión). Decidí usar esta parte de la canción porque los cambios en estos parámetros necesitan más tiempo para apreciarse, y las transiciones más pausadas de esta sección encajan perfectamente.

<img width="970" height="674" alt="image" src="https://github.com/user-attachments/assets/f85ee02c-a7be-482f-a4a4-a0acd719d118" /> <img width="711" height="494" alt="image" src="https://github.com/user-attachments/assets/9d5ebc06-f4b2-41dd-b875-6d75719a698c" /> <img width="846" height="587" alt="image" src="https://github.com/user-attachments/assets/69a90d4a-9d03-4723-89a9-1804a93c9e25" /> <img width="850" height="591" alt="image" src="https://github.com/user-attachments/assets/4a9580b3-90c2-4879-aa05-1de1e05a2df5" />

El regreso de esta parte a la melodía principal ocurre con una especie de explosión, lo cual es perfecto: el cambio brusco de separación y alineación de 0 a 50 hace que los renacuajos se dispersen como si estallaran, representando así ese momento musical.

La percusión se representará con las mismas gotas triangulares y ondas en forma de decágono que usé en la unidad anterior. Estas, además de dejar la onda en el agua, en esta ocasión también cambiarán el color de la laguna entre distintos tonos de azul.

<img width="593" height="415" alt="image" src="https://github.com/user-attachments/assets/995ac50a-6e9e-4dd8-a9ec-5ec74fb10b7b" /> <img width="585" height="404" alt="image" src="https://github.com/user-attachments/assets/1fcbe823-1a7f-4c62-b318-aed55e0cbc03" />

### Aplicación en ejecución
<img width="1892" height="1071" alt="image" src="https://github.com/user-attachments/assets/cb69de4e-d34c-4354-b574-c91e4eafff6d" />

``` js

let flock;
let sepDist = 25, aliDist = 50, cohDist = 50;
let wrapEnabled = true;

let targets = [];
let foodDrops = [];
let ripples = [];


const WATER_BASES = [
  [12, 54, 130],
  [10, 69, 150],
  [14, 84, 170],
  [8, 60, 140]
];
let waterIdx = 0;
let BG = WATER_BASES[waterIdx];

const PALETTE = [
  [255, 99, 71], [255, 184, 77], [80, 220, 130],
  [120, 200, 255], [200, 140, 255], [255, 220, 90],
  [80, 230, 210], [0,0,0]
];


let snd, fft, peak;
let audioReady = false, audioPlaying = false;
const PEAK_FREQ_LOW = 20, PEAK_FREQ_HIGH = 180;
const PEAK_THRESHOLD = 0.22, PEAK_COOLDOWN_MS = 110;
let lastPercAt = -9999;
const PERC_MARGIN = 170;

let particles = [];
const PARTICLE_LIFESPAN = 3380;
const RIPPLE_LIFESPAN   = 1000;
const PERC_LIFE_MS_MIN  = 120;
const PERC_LIFE_MS_MAX  = 160;
const SPEED_LINEAR = 1.0;
const ORBIT_MIN = 2, ORBIT_MAX = 5;


function preload(){ snd = loadSound('WMWT_P5.mp3'); }

function setup() {
  createCanvas(windowWidth, windowHeight);
  flock = new Flock();
  for (let i = 0; i < 150; i++) flock.addBoid(new Boid(random(width), random(height)));

  fft = new p5.FFT(0.9, 1024);
  peak = new p5.PeakDetect(PEAK_FREQ_LOW, PEAK_FREQ_HIGH, PEAK_THRESHOLD, 20);
  background(BG[0], BG[1], BG[2]);
}

function windowResized(){ resizeCanvas(windowWidth, windowHeight); }


function draw() {
  background(BG[0], BG[1], BG[2]);

  if (audioPlaying) {
    fft.analyze();
    peak.update(fft);
    detectPercussion();
  }

  flock.run();

  for (let f of foodDrops) { f.update(); f.display(); }
  for (let i = foodDrops.length - 1; i >= 0; i--) {
   if (foodDrops[i].isDone()) {
  const pos = foodDrops[i].pos.copy();
  ripples.push(new CircleRipple(pos.x, pos.y, foodDrops[i].col));
  targets.push({ 
    x: pos.x, 
    y: pos.y, 
    touched: false, 
    removeTime: null, 
    col: foodDrops[i].col  
  });
  foodDrops.splice(i, 1);
}
  }

  for (let r of ripples) { r.update(); r.display(); }
  for (let i = ripples.length - 1; i >= 0; i--) if (ripples[i].isDead()) ripples.splice(i, 1);

  for (let p of particles) { p.update(); p.display(); }
  for (let i = particles.length - 1; i >= 0; i--) {
    if (particles[i].isDead()) { particles[i].impact(); particles.splice(i, 1); }
  }

  noStroke(); fill(80, 220, 130);
 for (let t of targets) {
  noStroke();
  fill(t.col);             
  ellipse(t.x, t.y, 10, 10);
}
  let now = millis();
  targets = targets.filter(t => !(t.touched && now > t.removeTime));
}


function mousePressed() {
  foodDrops.push(new FoodDrop(mouseX, mouseY, 1000)); // caída 1 s
}

function keyPressed() {
  if (key === 't' || key === 'T') wrapEnabled = !wrapEnabled;

  if (key === '1') { sepDist=25; aliDist=50; cohDist=50; }
  if (key === '2') { sepDist=0;  aliDist=0;  cohDist=50; }
  if (key === '3') { sepDist=0;  aliDist=50; cohDist=50; }
  if (key === '4') { sepDist=10; aliDist=50; cohDist=50; }
  if (key === '6') { sepDist=100;aliDist=50; cohDist=50; }

  if (key === 'f' || key === 'F'){
    
    fullscreen(!fullscreen())
    ensureAudio(true); 
  } 
}


function ensureAudio(toggleOnly=false) {
  if (!audioReady) {
    userStartAudio().then(() => {
      fft.setInput(snd);
      audioReady = true;
      if (toggleOnly && !snd.isPlaying()) { snd.loop(); audioPlaying = true; }
    });
  } else {
    if (toggleOnly) {
      if (snd.isPlaying()) { snd.pause(); audioPlaying=false; }
      else { snd.loop(); audioPlaying=true; }
    }
  }
}

function detectPercussion() {
  const now = millis();
  if (peak.isDetected && now - lastPercAt > PEAK_COOLDOWN_MS) {
    lastPercAt = now;
    const pos = randomCorner(PERC_MARGIN);
    particles.push(new PercussionDrop(pos.x, pos.y));
    waterIdx = (waterIdx + 1) % WATER_BASES.length;
    BG = WATER_BASES[waterIdx];
  }
}

function randomCorner(margin = 0) {
  const m = min(margin, width * 0.25, height * 0.25);
  const i = floor(random(4));
  return [
    createVector(m, m),
    createVector(width - m, m),
    createVector(width - m, height - m),
    createVector(m, height - m),
  ][i];
}


class Flock {
  constructor(){ this.boids = []; }
  addBoid(b){ this.boids.push(b); }
  run(){ for (let b of this.boids) b.run(this.boids); }
}


class Boid {
  constructor(x, y) {
    this.acceleration = createVector(0, 0);
    this.velocity = p5.Vector.random2D().mult(random(1.2,2.2));
    this.position = createVector(x, y);
    this.r = 4;
    this.maxspeed = 4.2;     // más rápidos
    this.maxforce = 0.07;
    const c = random(PALETTE); this.fillCol = color(c[0],c[1],c[2]);
  }

  run(boids){
    const sep = this.separate(boids).mult(1.4);
    const ali = this.align(boids).mult(1.0);
    const coh = this.cohere(boids).mult(1.0);
    const tgt = this.seekTargets().mult(1.2);
    this.applyForce(sep); this.applyForce(ali); this.applyForce(coh); this.applyForce(tgt);
    this.update(); this.borders(); this.show();
  }

  applyForce(f){ this.acceleration.add(f); }
  update(){
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  borders(){
    if (wrapEnabled) {
      if (this.position.x < -this.r) this.position.x = width + this.r;
      if (this.position.y < -this.r) this.position.y = height + this.r;
      if (this.position.x > width + this.r) this.position.x = -this.r;
      if (this.position.y > height + this.r) this.position.y = -this.r;
    } else {
      if (this.position.x < this.r) { this.position.x=this.r; this.velocity.x*=-1; }
      if (this.position.x > width-this.r) { this.position.x=width-this.r; this.velocity.x*=-1; }
      if (this.position.y < this.r) { this.position.y=this.r; this.velocity.y*=-1; }
      if (this.position.y > height-this.r) { this.position.y=height-this.r; this.velocity.y*=-1; }
    }
  }

  seek(target){
    let desired = p5.Vector.sub(target, this.position).setMag(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    return steer;
  }

  seekTargets(){
    let force = createVector(0,0);
    for (let t of targets) {
      let d = dist(this.position.x, this.position.y, t.x, t.y);
      if (d < 10 && !t.touched) { t.touched = true; t.removeTime = millis()+1000; }
      else if (!t.touched) { force.add(this.seek(createVector(t.x, t.y))); }
    }
    return force;
  }

  show(){
    const ang = this.velocity.heading();
    push();
    translate(this.position.x, this.position.y);
    rotate(ang);
    noStroke();
    fill(this.fillCol);
    ellipse(0, 0, 12, 8);
    fill(255, 180);
    ellipse(-6, 0, 8, 5);
    pop();
  }

  separate(boids){
    let steer = createVector(0,0), count=0;
    for (let o of boids){
      let d = p5.Vector.dist(this.position, o.position);
      if (d > 0 && d < sepDist){
        let diff = p5.Vector.sub(this.position, o.position).normalize().div(d);
        steer.add(diff); count++;
      }
    }
    if (count>0) steer.div(count);
    if (steer.mag()>0) steer.normalize().mult(this.maxspeed).sub(this.velocity).limit(this.maxforce);
    return steer;
  }
  align(boids){
    let sum = createVector(0,0), count=0;
    for (let o of boids){
      let d = p5.Vector.dist(this.position, o.position);
      if (d>0 && d < aliDist){ sum.add(o.velocity); count++; }
    }
    if (count>0){ sum.div(count).normalize().mult(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.velocity).limit(this.maxforce); return steer; }
    return createVector(0,0);
  }
  cohere(boids){
    let sum = createVector(0,0), count=0;
    for (let o of boids){
      let d = p5.Vector.dist(this.position, o.position);
      if (d>0 && d<cohDist){ sum.add(o.position); count++; }
    }
    if (count>0){ sum.div(count); return this.seek(sum); }
    return createVector(0,0);
  }
}


class FoodDrop {
  constructor(x, y, durationMs){
    this.pos = createVector(x, y);
    this.birth = millis();
    this.duration = durationMs;
    this.rStart = 46; this.rEnd = 14;
    const c = random(PALETTE); this.col = color(c[0],c[1],c[2]);
    this.done = false;
  }
  progress(){ return constrain((millis()-this.birth)/this.duration,0,1); }
  update(){ if (this.progress()>=1 && !this.done) this.done = true; }
  display(){
    const k = this.progress();
    const r = lerp(this.rStart, this.rEnd, k);
    noStroke();
    fill(red(this.col), green(this.col), blue(this.col), 190);
    circle(this.pos.x, this.pos.y, r*2);
  }
  isDone(){ return this.done; }
}


class Ripple {
  constructor(x, y, col) { this.pos=createVector(x,y); this.birth=millis(); this.lifespan=RIPPLE_LIFESPAN; this.maxR=random(90,140); this.col=col||color(255); }
  t(){ return constrain((millis()-this.birth)/this.lifespan, 0, 1); }
  isDead(){ return millis()-this.birth >= this.lifespan; }
}
class CircleRipple extends Ripple {
  update(){}
  display(){
    const k=this.t(), r=this.maxR * (1 - (1-k)*(1-k));
    const a=map(1-k,0,1,0,230);
    noFill(); stroke(red(this.col),green(this.col),blue(this.col),a);
    strokeWeight(3); circle(this.pos.x,this.pos.y,r*2);
  }
}
class PolygonRipple extends Ripple {
  constructor(x,y,sides,col){ super(x,y,col); this.sides=sides; }
  update(){}
  display(){
    const k=this.t(), r=this.maxR * (1 - pow(1-k,3));
    const a=map(1-k,0,1,0,230);
    noFill(); stroke(red(this.col),green(this.col),blue(this.col),a);
    strokeWeight(4);
    push(); translate(this.pos.x,this.pos.y);
    beginShape(); for (let i=0;i<this.sides;i++){ const ang=TWO_PI*(i/this.sides); vertex(r*cos(ang), r*sin(ang)); }
    endShape(CLOSE); pop();
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.birth = millis();
    this.lifespan = PARTICLE_LIFESPAN;
    this.rMax = random(70, 110);
    this.rMin = random(6, 10);
    const c = random(PALETTE); this.col = color(c[0],c[1],c[2],220);
  }
  progress(){ return constrain((millis()-this.birth)/this.lifespan,0,1); }
  currentRadius(){ return lerp(this.rMax, this.rMin, this.progress()); }
  update(){}
  display(){ noStroke(); fill(this.col); circle(this.position.x,this.position.y,this.currentRadius()*2); }
  impact(){ ripples.push(new CircleRipple(this.position.x,this.position.y,this.col)); }
  isDead(){ return millis()-this.birth >= this.lifespan; }
}
class MelodyLinear extends Particle {
  constructor(x,y){ super(x,y); this.v = createVector(0, SPEED_LINEAR); }
  update(){ this.position.add(this.v); this.position.y = constrain(this.position.y,0,height); }
}
class MelodyCurved extends Particle {
  constructor(x,y){ super(x,y); this.origin=this.position.copy(); this.orbitR = random(ORBIT_MIN, ORBIT_MAX); this.theta=random(TWO_PI); this.angSpeed=random(0.06,0.12); }
  update(){ this.theta += this.angSpeed; this.position.x = this.origin.x + this.orbitR*cos(this.theta); this.position.y = this.origin.y + this.orbitR*sin(this.theta); }
}
class PercussionDrop extends Particle {
  constructor(x,y){ super(x,y); this.lifespan = random(PERC_LIFE_MS_MIN, PERC_LIFE_MS_MAX); this.rMax = random(80,130); this.rMin = random(8,12);
    const dir = p5.Vector.sub(createVector(width/2,height/2), this.position); this.angle = atan2(dir.y, dir.x); }
  display(){ const r=this.currentRadius(); noStroke(); fill(this.col); push(); translate(this.position.x,this.position.y); rotate(this.angle);
    triangle(-r*0.8,-r*0.6, -r*0.8,r*0.6, r,0); pop(); }
  impact(){ ripples.push(new PolygonRipple(this.position.x,this.position.y,10,this.col)); }
}


function randomCorner(margin = 0) {
  const m = min(margin, width * 0.25, height * 0.25);
  const i = floor(random(4));
  return [
    createVector(m, m),
    createVector(width - m, m),
    createVector(width - m, height - m),
    createVector(m, height - m),
  ][i];
}
function randomColor(alpha=255){ const c = random(PALETTE); return color(c[0],c[1],c[2],alpha); }

```

### Link: https://editor.p5js.org/JuanSMarin2/sketches/jPGB1DKOS

# Auto evaluación
## Nota: 5.0
Defensa de la nota

Actividad 01:
Seleccioné dos obras de Tyler Hobbs, las analicé y expliqué qué me llamó la atención de cada una. Reflexioné sobre cómo su proceso me inspira a experimentar con mis propios parámetros.

Actividad 02:
Expliqué con mis palabras qué es una steering force, su diferencia con otras fuerzas vistas antes y su relación con Craig Reynolds. La explicación fue clara y aplicada al contexto de agentes.

Actividad 03:
Expliqué la estructura del campo de flujo, cómo los agentes calculan sus fuerzas de dirección, listé los parámetros clave y mostré el código modificado con distribución gaussiana. Documenté los efectos observados con capturas de pantalla y explicación detallada.

Actividad 04:
Expliqué las tres reglas del flocking, listé parámetros clave, realicé una modificación en los radios de percepción y describí claramente el efecto (agrupación en cúmulo y órbitas tipo “Saturno”). Incluí código y evidencia visual.

Aplicación (Apply):
Propuse un proyecto inspirado en los renacuajos y la música de Persona 5. Documenté las decisiones creativas, cómo representé los momentos musicales con parámetros de flocking, gotas, ondas y cambios de color. Incluí código, capturas y justificación conceptual.

