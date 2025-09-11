# Evidencias de la unidad 5


## Ejemplo 4.2: an Array of Particles.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la distribución no uniforme para generar diferentes figuras, cada particula puede ser un circulo, cuadrado o triangulo pero tiene mas probabilidades de ser un circulo.

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
Cuando una particula supera su tiempo de vida es marcada como muerta y el metodo: particles.splice(i, 1); Elimina la particula actual (de la posición i) y solo elimina una. Para experimentar probé cambiando el segundo parametro del metodo, cuando se alcanza el tiempo limite de una particula las 5 particulas subsecuentes a la derecha son eliminadas lo que hace que se borren abruptamente sin llegar a aplicarse el efecto de desbanecerse.
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let particles = [];

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  particles.push(new Particle(width / 2, 20));

  // Looping through backwards to delete
  for (let i = particles.length - 1; i >= 0; i--) {
    let particle = particles[i];
    particle.run();
    if (particle.isDead()) {
      //remove the particle
      particles.splice(i, 50);
    }
  }
}
```


``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class


class Particle {
  constructor(x, y) {
      
    this.isCircle = false;
    this.isSquare = false;
    this.isTriangle = false;
    
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    
    const choice = floor(random(8));
    
    if(choice <= 2){
      this.isSquare = true;
    } else if (choice > 2 && choice <= 4){
      this.isTriangle = true;
    } else {
      this.isCircle = true;
    }
    
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    if(this.isCircle)
    {
        circle(this.position.x, this.position.y, 8);
    } else if(this.isSquare) {
      rect(this.position.x, this.position.y, 5,5);
    } else if (this.isTriangle) {
       triangle(this.position.x, this.position.y+3,
                this.position.x -3, this.position.y,
                this.position.x+3, this.position.y);
      
    }
  
  }

  // Is the particle still useful?
  isDead() {
    return (this.lifespan < 0.0);
  }
}

```

<img width="421" height="219" alt="image" src="https://github.com/user-attachments/assets/dd791e9a-f8f4-49e2-b2e4-70b436b5822d" />


### Link: https://editor.p5js.org/natureofcode/sketches/-xTbGZMim


## Ejemplo 4.4: a System of Systems.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Apliqué la resistencia del aire para reducir la velocidad en la que caen las particulas, hace un efecto interesante ya que las particulas no llegan tan abajo

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
La simulación original limita el tiempo de vida de las particulas igual que la anterior pero no limita el numero de emisores por lo que pueden existir demasiadas particulas al mismo tiempo y afectar al rendimiento, añadí un limite de 10 emisores para evitar esto.
``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Particles are generated each cycle through draw(),
// fall with gravity and fade out over time
// A ParticleSystem object manages a variable size
// list of particles.

// an array of ParticleSystems
let emitters = [];

function setup() {
  createCanvas(640, 240);
  let text = createP("click to add particle systems");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
    
    
    
  }
}

function mousePressed() {
  
  if(emitters.length < 10)
  emitters.push(new Emitter(mouseX, mouseY));
}
```

``` js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Simple Particle System

// A simple Particle class

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    let airResistance = createVector(0, -0.03);
    this.applyForce(gravity);
    this.applyForce(airResistance);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}

```
No cambié el codigo de la clase emittor

<img width="645" height="248" alt="image" src="https://github.com/user-attachments/assets/7b67570e-e418-444f-b822-37b21aa95819" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/hnZKSFoqz


