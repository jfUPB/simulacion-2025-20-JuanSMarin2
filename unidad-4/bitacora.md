# Evidencias de la unidad 4

## Explicación conceptual de la obra


* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Apliqué el concepto de ondas para emular el efecto de olas en la superficie del agua y resortes para disipar la fuerza de los cuadros a medida que se alejan del impacto

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Utilicé la resistencia del aire y de fluidos para variar la velocidad de caida de los circulos en base a si estan en el aire o en el agua. En aire uso un coeficiente menor y, dentro del líquido, un coeficiente mayor.

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Utilicé Motion 101 para aplicar el movimiento de los circulos.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Apliqué Random para el color de los circulos y distribución Gausseana para la generación de los circulos, haciendo que siempre aparezcan mas en el centro que en las esquinas.

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
> Con el clic se pueden activar y desactivar la generación de circulos y con el microfono se puede soplar para hacer una ola más grande.

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/JuanSMarin2/sketches/YQoa_REAf)


## Código de la obra 
sketch
``` js
let movers = [];
let liquid;
let waves = []; 
let surface = []; 

let isSpawning = false;
const SPAWN_EVERY_MS = 150; 
let lastSpawnAt = 0;

const G = 0.4;             
const AIR_C = 0.02;     
const LIQ_C = 0.25;        
const MASS = 5;             
const R_SPAWN = MASS * 8;   
const INITIAL_SPEED = 6;    

const SURFACE_SPACING = 20;
const K = 0.025;  
const DAMP = 0.92;
const SPREAD = 0.25; 


let mic;
let micReady = false;
const BLOW_THRESHOLD = 0.12;    
const BLOW_COOLDOWN_MS = 700;   
let lastBlowAt = -9999;
const GLOBAL_SPLASH_POWER = 22;  
const GLOBAL_SPLASH_SIGMA = 120; 

function setup() {
  createCanvas(640, 540);
  textFont('monospace');

  liquid = new Liquid(0, height / 2, width, height / 2, LIQ_C); 

  for (let x = 0; x <= width; x += SURFACE_SPACING) {
    surface.push({ x, y: liquid.y, offset: 0, vel: 0 });
  }

  // Prepara micrófono (se activará realmente en el primer clic)
  mic = new p5.AudioIn();
}

function draw() {
  background(255);
  liquid.show();

  // Detectar soplido
  detectBlowAndSplash();

  // Spawn automático controlado por toggle
  if (isSpawning && millis() - lastSpawnAt >= SPAWN_EVERY_MS) {
    spawnMoverGaussian();
    lastSpawnAt = millis();
  }

  for (let m of movers) {
    const gravity = createVector(0, G * m.mass);
    m.applyForce(gravity);

    const airDrag = quadraticDrag(m.velocity, AIR_C);
    m.applyForce(airDrag);

    if (liquid.contains(m)) {
      const liquidDrag = quadraticDrag(m.velocity, liquid.c);
      m.applyForce(liquidDrag);

      if (!m.inLiquid) {
        m.inLiquid = true;
        waves.push(new Wave(m.position.x, liquid.y));
        disturbSurface(m.position.x, m.velocity.y * 0.5);
      }
    } else {
      m.inLiquid = false;
    }

    m.update();
    m.bounceEdges();
    m.show();
  }

  movers = movers.filter(m => !m.isDead());

  updateSurface();
  showSurface();

  for (let i = waves.length - 1; i >= 0; i--) {
    waves[i].update();
    waves[i].show();
    if (waves[i].isDead()) waves.splice(i, 1);
  }

  drawHUD();
}

function mousePressed() {
  if (!micReady) {
    userStartAudio().then(() => {
      mic.start(() => { micReady = true; });
    });
  }
  // Toggle del spawn
  isSpawning = !isSpawning;
}


function spawnMoverGaussian() {
  let media = width / 2;
  let desviacion = width / 6.0;
  let x = randomGaussian() * desviacion + media;
  x = constrain(x, R_SPAWN, width - R_SPAWN);

  let y = random(R_SPAWN, height / 4);

  const m = new Mover(x, y, MASS);
  m.velocity.x = random(-2, 2);
  m.velocity.y = random(0, 2);
  movers.push(m);
}

function updateSurface() {
  // resorte básico
  for (let p of surface) {
    let force = -K * p.offset;
    p.vel += force;
    p.vel *= DAMP;
    p.offset += p.vel;
  }

  // propagación lateral
  let leftDeltas = [];
  let rightDeltas = [];

  for (let i = 0; i < surface.length; i++) {
    if (i > 0) {
      let delta = SPREAD * (surface[i].offset - surface[i - 1].offset);
      leftDeltas[i - 1] = delta;
    }
    if (i < surface.length - 1) {
      let delta = SPREAD * (surface[i].offset - surface[i + 1].offset);
      rightDeltas[i + 1] = delta;
    }
  }

  for (let i = 0; i < surface.length; i++) {
    if (i > 0) surface[i - 1].vel += leftDeltas[i - 1] || 0;
    if (i < surface.length - 1) surface[i + 1].vel += rightDeltas[i + 1] || 0;
  }
}

function showSurface() {
  noStroke();
  fill(50, 150, 255);
  for (let p of surface) {
    rectMode(CENTER);
    rect(p.x, p.y + p.offset, SURFACE_SPACING - 2, 10);
  }
}

function disturbSurface(x, power) {
  let idx = floor(x / SURFACE_SPACING);
  if (idx >= 0 && idx < surface.length) {
    surface[idx].vel += power;
  }
}

function drawHUD() {
  noStroke();
  fill(20);
  textSize(12);
  text("Movers: " + movers.length, 10, 20);
  if (micReady) {
    text("Mic level: " + nf(mic.getLevel(),1,3), 10, 40);
  }
}



function detectBlowAndSplash() {
  if (!micReady) return;

  const level = mic.getLevel(); // RMS 0..1
  const t = millis();

  if (level > BLOW_THRESHOLD && (t - lastBlowAt) > BLOW_COOLDOWN_MS) {
    lastBlowAt = t;

    const strength = constrain(map(level, BLOW_THRESHOLD, 0.6, 1, 2.6), 1, 3);
    const power = GLOBAL_SPLASH_POWER * strength;

    const impactX = width / 2;
    globalSplash(impactX, power, GLOBAL_SPLASH_SIGMA);

    waves.push(new Wave(impactX, liquid.y));
    waves[waves.length - 1].growth = 4;
  }
}

function globalSplash(cx, power, sigma) {
  const twoSigma2 = 2 * sigma * sigma;
  for (let i = 0; i < surface.length; i++) {
    const dx = surface[i].x - cx;
    const gauss = Math.exp(-(dx * dx) / twoSigma2);
    surface[i].vel += power * gauss;
  }
}


function quadraticDrag(vel, c) {
  const speed = vel.mag();
  if (speed === 0) return createVector(0, 0);
  const dragMag = c * speed * speed;
  const drag = vel.copy().mult(-1);
  drag.setMag(dragMag);
  return drag;
}
```
Liquid
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
    fill(135, 206, 235);
    rectMode(CORNER); 
    rect(this.x, this.y, this.w, this.h);
  }
}
```
Mover
``` js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.birth = millis();
    this.lifespan = 10000;
    this.col = color(random(255), random(255), random(255), 220);
    this.inLiquid = false;
  }

  isDead() {
    return (millis() - this.birth) > this.lifespan;
  }

  applyForce(force) {
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
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    }
    if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
    if (this.position.y < this.radius) {
      this.position.y = this.radius;
      this.velocity.y *= bounce;
    }
  }
}
```
Wave
``` js
class Wave {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.r = 0;
    this.alpha = 200;
    this.growth = 2;
  }

  update() {
    this.r += this.growth;
    this.alpha -= 3;
  }

  show() {
    noFill();
    stroke(0, 100, 255, this.alpha);
    strokeWeight(2);
    ellipse(this.x, this.y, this.r * 2);
  }

  isDead() {
    return this.alpha <= 0;
  }
}

```

## Captura de pantalla representativa
<img width="630" height="516" alt="image" src="https://github.com/user-attachments/assets/a9937962-9cd6-4013-8ae6-b6bbd4c53438" />







