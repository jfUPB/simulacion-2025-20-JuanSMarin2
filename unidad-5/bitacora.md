# Evidencias de la unidad 5


## Ejemplo 4.2: an Array of Particles.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la distribución no uniforme para generar diferentes figuras, cada particula puede ser un circulo, cuadrado o triangulo pero tiene mas probabilidades de ser un circulo. Utilicé random(10) para obtener un valor entre 0 y 9. Siendo que solo los valores 0, 1, 2 activan el cuadrado y 3, 4 el triángulo y el resto activa el círculo, lo que da una mayor probabilidad

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
    
    const choice = floor(random(10));
    
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
Apliqué la resistencia del aire para reducir la velocidad en la que caen las particulas, hace un efecto interesante ya que las particulas no llegan tan abajo. La fuerza de resistencia del aire es una fuerza constante hacia arriba (createVector(0, -0.03)) que contrarresta parcialmente la gravedad (createVector(0, 0.05)).

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



## Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la interpolación. Añadí un nuevo tipo de particula llamado Colorling y un nuevo metodo llamado ChooseColor. Las particulas base en ChooseColor escogen su color como una interpolación de los colores de los 2 Colorlings mas cercanos, si no hay ninguno se ponen blancas. Los Colorlings sobre escriben este metodo y escogen su color de forma aleatoria.

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
La simulación hace lo mismo que las anteriores, aparte de esto algo que no he mencionado es que la opacidad de las particulas disminuye con su tiempo de vida, aprovechando esto aumenté el tiempo de vida de mis colorlings para que no pierdan opacidad mientras estan en pantalla y resalten mas.

``` js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.4) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else if (r < 0.8) {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new Colorling(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run(this.particles); 
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.col = color(255); 
  }

  run(particles) {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.chooseColor(particles); 
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

 
  chooseColor(particles) {
    let colorlings = particles.filter(p => p instanceof Colorling);

    if (colorlings.length === 0) {
      this.col = color(255); 
      return;
    }

  
    colorlings.sort((a, b) =>
      p5.Vector.dist(this.position, a.position) -
      p5.Vector.dist(this.position, b.position)
    );

    if (colorlings.length === 1) {
      this.col = colorlings[0].myColor;
    } else {
      let c1 = colorlings[0].myColor;
      let c2 = colorlings[1].myColor;
      this.col = lerpColor(c1, c2, 0.5); 
    }
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);

    rectMode(CENTER);
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);

    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}
```
``` js


class Colorling extends Particle {
  constructor(x, y) {
    super(x, y);
    this.myColor = color(random(255), random(255), random(255));
    this.col = this.myColor; 
    
    
     this.lifespan = 500.0;
  }

  chooseColor(particles) {
    this.col = this.myColor;
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(3);
    fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);
    ellipse(this.position.x, this.position.y, 16);
  }
}


```
<img width="303" height="234" alt="image" src="https://github.com/user-attachments/assets/1bd98790-41f0-4561-8674-187e7f7f1ee2" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/lYpqX6gP9h


