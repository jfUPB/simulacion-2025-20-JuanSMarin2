# Evidencias de la unidad 5


## Ejemplo 4.2: an Array of Particles.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la distribución no uniforme para generar diferentes figuras, cada particula puede ser un circulo, cuadrado o triangulo pero tiene mas probabilidades de ser un circulo. Utilicé random(10) para obtener un valor entre 0 y 9. Siendo que solo los valores 0, 1, 2 activan el cuadrado y 3, 4 el triángulo y el resto activa el círculo, lo que da una mayor probabilidad

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
Cuando una particula supera su tiempo de vida es marcada como muerta y el metodo: particles.splice(i, 1); Elimina la particula actual (de la posición i) y solo elimina una. Para experimentar probé cambiando el segundo parametro del metodo, cuando se alcanza el tiempo limite de una particula las 5 particulas subsecuentes a la derecha son eliminadas lo que hace que se borren abruptamente sin llegar a aplicarse el efecto de desbanecerse.
Al borrar partículas con splice, no solo desaparecen visualmente, también cambia el tamaño del arreglo, y eso influye en cómo sigue funcionando el sistema y en el rendimiento de la simulación.
Splice no libera memoria por sí solo, lo que hace es romper las referencias; el recolector de basura se encarga de limpiar cuando corresponde.

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
Apliqué la resistencia del aire para reducir la velocidad en la que caen las particulas, hace un efecto interesante ya que las particulas no llegan tan abajo. Para calcularla, hago que dependa de la velocidad de cada partícula y que siempre actúe en dirección contraria a su movimiento. Esto hace que cuando una partícula cae rápido, la resistencia sea más fuerte, y cuando cae lento casi no se note.
La resistencia hace que las partículas se acumulen más tiempo en pantalla, y si no limito los emisores, el sistema se llena demasiado. Es decir, la fuerza aplicada se conecta directamente con cuántas partículas puede manejar el sistema sin problemas por eso debo limitar las particulas.

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
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);

   
    let drag = this.getAirDrag(0.01); 
    this.applyForce(drag);

    this.update();
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

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }

  // Método para calcular resistencia cuadrática
  getAirDrag(c) {
    let speed = this.velocity.mag();
    if (speed === 0) return createVector(0, 0);

    let dragMagnitude = c * speed * speed;
    let drag = this.velocity.copy();
    drag.mult(-1);
    drag.setMag(dragMagnitude);
    return drag;
  }
}


```
No cambié el codigo de la clase emittor

<img width="645" height="248" alt="image" src="https://github.com/user-attachments/assets/7b67570e-e418-444f-b822-37b21aa95819" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/hnZKSFoqz



## Ejemplo 4.5: a Particle System with Inheritance and Polymorphism.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la interpolación. Añadí un nuevo tipo de particula llamado Colorling y un nuevo metodo llamado ChooseColor. Las particulas base en ChooseColor escogen su color como una interpolación de los colores de los 2 Colorlings mas cercanos, si no hay ninguno se ponen blancas. Los Colorlings sobreescriben este metodo y escogen su color de forma aleatoria.

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
La simulación hace lo mismo que las anteriores, la opacidad de las particulas disminuye con su tiempo de vida en base al parametro lifespan que cambia el alpha en fill y stroke como tercer parametro, aprovechando esto aumenté el tiempo de vida de mis colorlings para que no pierdan opacidad mientras estan en pantalla y resalten mas.

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


## Ejemplo 4.6: a Particle System with Forces.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Combiné este ejercicio con el ejemplo del péndulo de la unidad pasada. El emisor se coloca en el centro del péndulo y se mueve junto con él, de modo que las partículas oscilan siguiendo el movimiento del péndulo. Esto ocurre porque las coordenadas del emisor dependen directamente del ángulo y la posición calculados del péndulo y el emisor toma esa misma posición como origen.

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
La particula disminuye su tiempo de vida como siempre cada frame, cuando llega a 0 se considera muerta y se elimina. Añadí saltos de levy con (random(1) < 0.01) 1% de probabilidad cada frame de que ocurra un salto. Cada salto puede restar de golpe entre 30 y 80 del tiempo de vida de la particula haciendo que muera mucho antes. 

``` js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
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

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);

  
    this.lifespan -= 2;


    if (random(1) < 0.01) { 
      let jump = random(30, 80); 
      this.lifespan -= jump;
    }

    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}
class Emitter {
  constructor(x, y, r = 150) {
    this.pendulum = new Pendulum(x, y, r); 
    this.particles = [];
  }

  addParticle() {

    let origin = this.pendulum.bob;
    this.particles.push(new Particle(origin.x, origin.y));
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  run() {

    this.pendulum.update();
    this.pendulum.drag();
    this.pendulum.show();


    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }


  clicked(mx, my) {
    this.pendulum.clicked(mx, my);
  }

  stopDragging() {
    this.pendulum.stopDragging();
  }
}
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Pendulum

// A Simple Pendulum Class

// This constructor could be improved to allow a greater variety of pendulums
class Pendulum {
  constructor(x, y, r) {
    // Fill all variables
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995; // Arbitrary damping
    this.ballr = 24.0; // Arbitrary ball radius
  }

  // Function to update position
  update() {
    // As long as we aren't dragging the pendulum, let it swing!
    if (!this.dragging) {
      let gravity = 0.4; // Arbitrary constant
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle); // Calculate acceleration (see: http://www.myphysicslab.com/pendulum1.html)

      this.angleVelocity += this.angleAcceleration; // Increment velocity
      this.angle += this.angleVelocity; // Increment angle

      this.angleVelocity *= this.damping; // Apply some damping
    }
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle), 0); // Polar to cartesian conversion
    this.bob.add(this.pivot); // Make sure the position is relative to the pendulum's origin

    stroke(0);
    strokeWeight(2);
    // Draw the arm
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);
    fill(127);
    // Draw the ball
    circle(this.bob.x, this.bob.y, this.ballr * 2);
  }

  // The methods below are for mouse interaction

  // This checks to see if we clicked on the pendulum ball
  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  // This tells us we are not longer clicking on the ball
  stopDragging() {
    this.angleVelocity = 0; // No velocity once you let go
    this.dragging = false;
  }

  drag() {
    // If we are draging the ball, we calculate the angle between the
    // pendulum origin and mouse position
    // we assign that angle to the pendulum
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY)); // Difference between 2 points
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90); // Angle relative to vertical axis
    }
  }
}
let emitter;

function setup() {
  createCanvas(550, 480);
  emitter = new Emitter(width / 2, 50); 
}

function draw() {
  background(255,30);

  // Apply gravity force to all Particles
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  emitter.addParticle();
  emitter.run();
}

function mousePressed() {
  emitter.clicked(mouseX, mouseY);
}

function mouseReleased() {
  emitter.stopDragging();
}

```

<img width="511" height="455" alt="image" src="https://github.com/user-attachments/assets/cec50fdb-1768-4e73-80d7-2a76f73de2ec" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/ScI2t148b


## Ejemplo 4.7: a Particle System with a Repeller.
### Explica qué concepto aplicaste, cómo lo aplicaste y por qué.
Utilicé la atracción gravitacional. se calcula un vector desde el emisor hasta la partícula, se mide su distancia y se limita con constrain. Luego se obtiene la intensidad de la fuerza con power / (distancia²) y se le aplica un signo: negativo si está en modo repeler o positivo si está en modo atractor. Para cambiar de modo entre positivo y negativo se usa el click del mouse y este se mueve en base a la posición del mouse. La fuerza que se aplica depende de la distancia entre el punto y cada partícula. 

### Vas a gestionar la creación y la desaparición de las partículas y la memoria. Explica cómo lo hiciste (aunque es posible que la simulación ya lo haga, trata de identificarlo de nuevo y explicarlo con tus palabras).
Hice que al presionar el enter se borren todas las particulas con: 
  if (keyCode === ENTER) {
 emitter.particles.splice(0, emitter.particles.length);
  }
Se borra desde la posición 0 todas las n particulas hacia la derecha, de esta forma se puede regular el sistema por si se acumulan muchas particulas alrededor del atractor y evitar problemas de rendimiento. El programa sigue manteniendo el mismo sistema de tiempo de vida y eliminación de los ejercicios anteriores.

``` js
// One ParticleSystem
let emitter;

//{!1} One repeller
let repeller;

function setup() {
  createCanvas(640 , 240);
  emitter = new Emitter(width / 2, 60);
  repeller = new Repeller(width / 2, 250);
}

function draw() {
  background(255);
  emitter.addParticle();
  // We’re applying a universal gravity.
  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);
  //{!1} Applying the repeller
  emitter.applyRepeller(repeller);
  emitter.run();

  
   repeller.update();
  repeller.show();
}

function mousePressed() {
  repeller.toggleMode(); 
}
function keyPressed() {
 

  if (keyCode === ENTER) {
 emitter.particles.splice(0, emitter.particles.length);
  }
}
```
``` js
//{!1} The Emitter manages all the particles.
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Applying a force as a p5.Vector
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  applyRepeller(repeller) {
    //{!4} Calculating a force for each Particle based on a Repeller
    for (let particle of this.particles) {
      let force = repeller.repel(particle);
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
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
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
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
``` js
class Repeller {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.power = 150;
    this.isAttractor = false; 
  }

  update() {

    this.position.set(mouseX, mouseY);
  }

  toggleMode() {
    this.isAttractor = !this.isAttractor;
  }

  show() {
    stroke(0);
    strokeWeight(2);
    if (this.isAttractor) {
      fill(255, 0, 0); // rojo para atractor
    } else {
      fill(0, 0, 255); // azul para repeler
    }
    circle(this.position.x, this.position.y, 32);
  }

  repel(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);

    // si es atractor, la fuerza es positiva, si no negativa
    let sign = this.isAttractor ? 1 : -1;
    let strength = (sign * this.power) / (distance * distance);

    force.setMag(strength);
    return force;
  }
}

```

<img width="636" height="232" alt="image" src="https://github.com/user-attachments/assets/dd7af6a5-9270-4da3-86bc-253dfa0cdc1c" />
<img width="603" height="233" alt="image" src="https://github.com/user-attachments/assets/d7e9c4e5-b78d-4c94-81e8-4f4ca2b770cd" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/c1CApjubN


