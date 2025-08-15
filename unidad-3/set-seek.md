# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 05 ¿Y qué relación tiene esto de las leyes de Newton con al arte generativo?

``` js
applyForce(force) {
  this.acceleration = force;
}
```
### ¿Qué problema le ves a este planteamiento?
Se esta asignando el valor de la fuerza a la aceleración en vez de sumarlo por lo que al poner otra fuerza se sobreescribe la anterior

### ¿Qué solución propones?
Que el metodo sume la fuerza nueva a la aceleración

### ¿Cómo lo implementarías en p5.js?
Con la función add
this,acceleration.add(force);

### Actividad 06
### ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
Para que la aceleración no siga aumentando infinitamente en cada frame.

# Actividad 09: Modelando fuerzas
## Fricción
``` js
let movers = [];
let spawnSide = 'left';          
const SPAWN_MARGIN = 30;
const MASS = 5;


let isSpawning = false;
const SPAWN_EVERY_MS = 150;      
let lastSpawnAt = 0;

function setup() {
  createCanvas(640, 240);
  textFont('monospace');
}

function draw() {
  background(255);


  if (isSpawning && millis() - lastSpawnAt >= SPAWN_EVERY_MS) {
    spawnMover();
    lastSpawnAt = millis();
  }

  // Fuerzas
  let gravity = createVector(0, 1);


  for (let m of movers) {
    m.applyForce(gravity);

    if (m.contactEdge()) {

      let c = 0.1;
      let friction = m.velocity.copy().mult(-1);
      friction.setMag(c);
      m.applyForce(friction);
    }

    m.bounceEdges();
    m.update();
    m.show();
  }

 
  movers = movers.filter(m => !m.isDead());

  // HUD
  noStroke();
  fill(20);
  textSize(12);
  text(
    `Click: ${isSpawning ? 'PARAR' : 'LANZAR'}  |  Flechas ←/→ para cambiar lado  |  Lado: ${spawnSide.toUpperCase()}  |  Activas: ${movers.length}`,
    10, 20
  );
}

function mousePressed() {
  // Toggle
  isSpawning = !isSpawning;
  // Lanzamos una inmediata al activar
  if (isSpawning) {
    spawnMover();
    lastSpawnAt = millis();
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) spawnSide = 'right';
  else if (keyCode === LEFT_ARROW) spawnSide = 'left';
}

function spawnMover() {
  let x = (spawnSide === 'left') ? SPAWN_MARGIN : width - SPAWN_MARGIN;
  let y = SPAWN_MARGIN;

  let m = new Mover(x, y, MASS);

  let dir = (spawnSide === 'left') ? 1 : -1;

  let impulse = createVector(dir * 5, 0);
  m.applyForce(impulse);



  movers.push(m);
}
```
``` js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);

    this.birth = millis();       // para vida útil
    this.lifespan = 10000;       // 10 segundos
    this.col = color(random(255), random(255), random(255)); // color aleatorio
  }

  isDead() {
    return (millis() - this.birth) > this.lifespan;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(1.5);
    fill(this.col);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  contactEdge() {
    return (this.position.y > height - this.radius - 1);
  }

  bounceEdges() {
    let bounce = -0.9;
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
  }
}

```

### Explica cómo modelaste cada fuerza.
Tome el ejemplo que ya habia en el libro y lo modifique, este aplicaba la friccion cada que la particula esta a un pixel de un borde de la pantalla con una fuerza en dirección contraria al movimiento y magnitud constante c

La fuerza de propulsión simplemente es disparada en X en la dirección seleccionada y una magnitud predefinida: F = ( +/- impulseMag, 0 )

### Conceptualmente cómo se relaciona la fuerza con la obra generativa.
Se generan muchos circulos que gracias a la fricción siguen un recorrido como una espiral con cada circulo de color aleatorio, el ususario puede interactuar con el programa activando o desactivando la generación y eligiendo el lado del que salen

<img width="634" height="235" alt="image" src="https://github.com/user-attachments/assets/f30c8c95-a98f-48eb-a8cb-be4320b78bec" />
<img width="640" height="235" alt="image" src="https://github.com/user-attachments/assets/0641c77f-d756-4e6a-b88b-3e3bf0188f18" />


### Link: https://editor.p5js.org/JuanSMarin2/sketches/MXbcuPh1F



## Resistencia del aire y de fluidos.
``` js
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x; this.y = y; this.w = w; this.h = h; this.c = c;
  }
  contains(m) {
    const p = m.position;
    return (p.x > this.x && p.x < this.x + this.w && p.y > this.y && p.y < this.y + this.h);
  }
  show() {
    noStroke();
    fill(200, 220, 255, 140);
    rect(this.x, this.y, this.w, this.h);
  }
}

// ---------- Utilidad: drag cuadrático ----------
function quadraticDrag(vel, c) {
  // |Fd| = c * v^2 ; dir = -v̂
  const speed = vel.mag();
  if (speed === 0) return createVector(0, 0);
  const dragMag = c * speed * speed;
  const drag = vel.copy().mult(-1);
  drag.setMag(dragMag);
  return drag;
}

```
``` js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);

    this.birth = millis();
    this.lifespan = 10000; // 10 s
    this.col = color(random(255), random(255), random(255), 220);
  }

  isDead() {
    return (millis() - this.birth) > this.lifespan;
  }

  applyForce(force) {
    // a = F/m
    const f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(1.2);
    fill(this.col);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  bounceEdges() {
    const bounce = -0.9;

    // derecha
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    }
    // izquierda
    if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    // suelo
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
    // techo
    if (this.position.y < this.radius) {
      this.position.y = this.radius;
      this.velocity.y *= bounce;
    }
  }
}
```
``` js
let movers = [];
let liquid;

// Toggle y emisión
let isSpawning = false;
let spawnSide = 'right';    
const SPAWN_EVERY_MS = 150; 
let lastSpawnAt = 0;


const G = 0.4;             
const AIR_C = 0.02;     
const LIQ_C = 0.25;        
const MASS = 5;             
const R_SPAWN = MASS * 8;   
const INITIAL_SPEED = 6;    

function setup() {
  createCanvas(640, 240);
  textFont('monospace');
  // Líquido: mitad inferior
  liquid = new Liquid(0, height / 2, width, height / 2, LIQ_C);
}

function draw() {
  background(255);


  liquid.show();

 
  if (isSpawning && millis() - lastSpawnAt >= SPAWN_EVERY_MS) {
    spawnMoverAtMouse();
    lastSpawnAt = millis();
  }


  for (let m of movers) {
    // Gravedad (F = m*g) -> a = g
    const gravity = createVector(0, G * m.mass);
    m.applyForce(gravity);

    // Resistencia del aire (siempre)
    const airDrag = quadraticDrag(m.velocity, AIR_C);
    m.applyForce(airDrag);

    // Resistencia adicional en el líquido
    if (liquid.contains(m)) {
      const liquidDrag = quadraticDrag(m.velocity, liquid.c);
      m.applyForce(liquidDrag);
    }

    m.update();
    m.bounceEdges();
    m.show();
  }

  // Eliminar partículas vencidas
  movers = movers.filter(m => !m.isDead());

  // Indicador del punto de lanzamiento (en el mouse)
  drawReticle();

  // HUD
  drawHUD();
}

function drawHUD() {
  noStroke();
  fill(20);
  textSize(12);
  text(
    `Click: ${isSpawning ? 'PARAR' : 'LANZAR'}  |  ←/→ cambia dirección inicial (${spawnSide.toUpperCase()})  |  Activas: ${movers.length}
Aire c=${AIR_C}  |  Líquido c=${LIQ_C}  |  Vida=10s  |  Spawn en mouse`,
    10, 18
  );
}

function drawReticle() {
  // Asegurar que el indicador quede dentro del lienzo
  const cx = constrain(mouseX, R_SPAWN, width - R_SPAWN);
  const cy = constrain(mouseY, R_SPAWN, height - R_SPAWN);
  noFill();
  stroke(0, 90);
  circle(cx, cy, 2 * R_SPAWN);
  // pequeña cruz
  line(cx - 6, cy, cx + 6, cy);
  line(cx, cy - 6, cx, cy + 6);
}

function mousePressed() {
  isSpawning = !isSpawning;
  if (isSpawning) {
    spawnMoverAtMouse();
    lastSpawnAt = millis();
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) spawnSide = 'right';
  else if (keyCode === LEFT_ARROW) spawnSide = 'left';
}

function spawnMoverAtMouse() {
  // Constrain para evitar spawns pegados al borde
  const x = constrain(mouseX, R_SPAWN, width - R_SPAWN);
  const y = constrain(mouseY, R_SPAWN, height - R_SPAWN);

  const m = new Mover(x, y, MASS);

  // Velocidad inicial horizontal según flecha activa
  const dir = (spawnSide === 'left') ? -1 : 1;
  m.velocity.x = dir * INITIAL_SPEED;

  movers.push(m);
}
```

### Explica cómo modelaste cada fuerza.
Para este ejercicio combine el ejemplo del libro con el ejercicio anterior de fricción, la resistencia del aire se calcula como en el ejemplo: Se multiplica la velocidad por un coeficiente, se multiplica por -1 para que vaya en la dirección opuesta y se aplica esta fuerza. Para la resistencia del liquido es lo mismo pero con coeficiente mayor.
### Conceptualmente cómo se relaciona la fuerza con la obra generativa.
Las esferas aparecen en la posición del mouse y son disparadas en la dirección escogida por las flechas como en el ejemplo anterior, siguen siendo infinitas pero se pueden activar o desactivar con click, su color es aleatorio.

<img width="628" height="232" alt="image" src="https://github.com/user-attachments/assets/1e916fa8-2e47-4b4a-95eb-d6da93ec2a13" />


### Link: https://editor.p5js.org/JuanSMarin2/sketches/bDakN0O4v

## Atracción gravitacional.

``` js
let G = 1.2;


class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20; 
    this.dragOffset = createVector(0, 0);
    this.dragging = false;
    this.rollover = false;
  }

  attract(mover) {

    let force = p5.Vector.sub(this.position, mover.position);
    let distance = force.mag();

    distance = constrain(distance, 5, 25);
    // |F| = G * M * m / r^2
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  show() {
    strokeWeight(4);
    stroke(0);
    if (this.dragging) fill(50);
    else if (this.rollover) fill(100);
    else fill(175, 200);
    circle(this.position.x, this.position.y, this.mass * 2);
  }

  handlePress(mx, my) {
    const d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.set(this.position.x - mx, this.position.y - my);
    }
  }

  handleHover(mx, my) {
    const d = dist(mx, my, this.position.x, this.position.y);
    this.rollover = d < this.mass;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.set(mx + this.dragOffset.x, my + this.dragOffset.y);
    }
  }

  stopDragging() {
    this.dragging = false;
  }
}
```
``` js
class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = 4; 
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);

    this.birth = millis();
    this.lifespan = 10000; // 10 s
    this.col = color(random(255), random(255), random(255), 220);
  }

  isDead() {
    return millis() - this.birth > this.lifespan;
  }

  applyForce(force) {
    // a = F / m
    const f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(1);
    fill(this.col);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  bounceEdges() {
    const bounce = -0.9;
    // Der
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    }
    // Izq
    if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    // Suelo
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
    // Techo
    if (this.position.y < this.radius) {
      this.position.y = this.radius;
      this.velocity.y *= bounce;
    }
  }
}
```
``` js
let movers = [];
let attractor;

// Toggle y esquina activa
let isSpawning = false;
let spawnSide = 'left'; 
const SPAWN_EVERY_MS = 120;
let lastSpawnAt = 0;

// Parámetros de spawn
const MASS = 1;       
const SPAWN_MARGIN = 20; 
const INITIAL_SPEED = 1.5;  

function setup() {
  createCanvas(640, 240);
  textFont('monospace');
  attractor = new Attractor();
}

function draw() {
  background(255);


  if (isSpawning && millis() - lastSpawnAt >= SPAWN_EVERY_MS) {
    spawnFromCorner();
    lastSpawnAt = millis();
  }


  for (let m of movers) {
    const Fg = attractor.attract(m);
    m.applyForce(Fg);

    m.update();
    m.bounceEdges();
    m.show();
  }

  // Limpiar partículas vencidas
  movers = movers.filter(m => !m.isDead());

  // Dibujar atractor y HUD
  attractor.show();
  drawHUD();
}

function drawHUD() {
  noStroke();
  fill(20);
  textSize(12);
  text(
    `Click: ${isSpawning ? 'PARAR' : 'LANZAR'}  |  ←/→ cambia esquina (${spawnSide.toUpperCase()})  |  Activas: ${movers.length}  |  G=${G}`,
    10, 18
  );
}

function spawnFromCorner() {
  // Esquinas superiores
  const x = (spawnSide === 'left') ? SPAWN_MARGIN : width - SPAWN_MARGIN;
  const y = SPAWN_MARGIN;

  const m = new Mover(x, y, MASS);


  const dir = (spawnSide === 'left') ? 1 : -1;
  m.velocity.x = dir * INITIAL_SPEED;
  m.velocity.y = random(-1.0, 1.0); 

  movers.push(m);
}


function mouseMoved() {
  attractor.handleHover(mouseX, mouseY);
}

function mousePressed() {

  attractor.handlePress(mouseX, mouseY);
  if (!attractor.dragging) {
    isSpawning = !isSpawning;
    if (isSpawning) {
      spawnFromCorner();
      lastSpawnAt = millis();
    }
  }
}

function mouseDragged() {
  attractor.handleHover(mouseX, mouseY);
  attractor.handleDrag(mouseX, mouseY);
}

function mouseReleased() {
  attractor.stopDragging();
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) spawnSide = 'right';
  else if (keyCode === LEFT_ARROW) spawnSide = 'left';
}
```

### Explica cómo modelaste cada fuerza.
Al igual que el ejemplo del libro se calcula la dirección de las particulas al atractor y la distancia con la resta de los vectores de posicion del atractor y la particula.
Despues se multiplican las masas por la constante gravitacional, que puede ser la que uno quiera y se divide por la distancia al cuadrado, despues se aplica esta fuerza a las particulas

### Conceptualmente cómo se relaciona la fuerza con la obra generativa.
Los tres ejercicios son variaciones de un mismo concepto, se puede escoger la deireccion de origen de las particulas y se disparan, estas se ven atraidas a un atractor que puede moverse con el mouse, en esencia combine los ejercicios anteriores con el ejemplo de atractor con muchos movers del libro.

Los colores siguen siendo aleatorios.





<img width="634" height="235" alt="image" src="https://github.com/user-attachments/assets/b93d8b01-e361-4a57-9a0d-d67bc6771fb5" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/kGItHOLmu



