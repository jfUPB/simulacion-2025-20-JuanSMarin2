# Unidad 2
## 🛠 Fase: Apply

Voy a hacer un programa con un mover con velocidad y aceleración aleatoria, cada cierto tiempo pueden pegar un aumento abrupto de velocidad con levy fligth, con la flecha derecha se puede aumentar manualmente el limite de velocidad y la aceleración y con la izquierda reducirlos, el cuadrante de la pantalla de arriba a la derecha (representado con un cuadrado roja) es una zona de alta fricción que reduce poco a poco la aceleración y abajo a la izquierda es una zona resbaladiza que las aumenta. Con clic se resetea la aplicacion

``` js
let movers = [];
let maxSpeed = 5;
let maxForce = 0.2;

function setup() {
  createCanvas(640, 480);
  resetMovers();
}

function draw() {
  background(255);


  fill(255, 0, 0, 100);
  noStroke();
  rect(width / 2, 0, width / 2, height / 2);


  fill(0, 255, 0, 100);
  noStroke();
  rect(0, height / 2, width / 2, height / 2);


  for (let mover of movers) {
    mover.update();
    mover.checkEdges();
    mover.show();
    mover.checkZones();
  }
}

function resetMovers() {
  movers = [];
  let colors = [
    color(255, 0, 0),   
    color(0, 255, 0),    
    color(0, 0, 255),    
    color(255, 165, 0),  
    color(128, 0, 128)  
  ];

  for (let i = 0; i < 5; i++) {
    movers.push(new Mover(colors[i]));
  }
}

function mousePressed() {
  resetMovers();
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) {
    maxSpeed += 1;
    maxForce += 2;
    for (let mover of movers) {
      mover.changeSpeed(2);
    }
  } else if (keyCode === LEFT_ARROW) {
    maxSpeed = max(1, maxSpeed - 1);
    maxForce = max(0.05, maxForce - 0.05);
    for (let mover of movers) {
      mover.changeSpeed(0.98);
    }
  }
}
```

``` js
class Mover {
  constructor(col) {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D().mult(random(1, 3));
    this.acceleration = p5.Vector.random2D().mult(random(0.1, 0.3));
    this.col = col;
  }

  update() {
    if (random(1) < 0.01) {
      let levy = p5.Vector.random2D().mult(random(20, 50));
      this.velocity.add(levy);
    }

    let randomAccel = p5.Vector.random2D().mult(random(0.05, maxForce));
    this.acceleration.add(randomAccel);
    this.acceleration.limit(maxForce);

    this.velocity.add(this.acceleration);
    this.velocity.limit(maxSpeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  changeSpeed(factor) {
    this.velocity.mult(factor);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(this.col);
    circle(this.position.x, this.position.y, 32);
  }

  checkEdges() {
    if (this.position.x > width) this.position.x = 0;
    else if (this.position.x < 0) this.position.x = width;

    if (this.position.y > height) this.position.y = 0;
    else if (this.position.y < 0) this.position.y = height;
  }

  checkZones() {
    if (this.position.x > width / 2 && this.position.y < height / 2) {
      this.velocity.mult(0.95);
    }
    if (this.position.x < width / 2 && this.position.y > height / 2) {
      this.velocity.mult(1.05);
    }
  }
}
```

<img width="633" height="470" alt="image" src="https://github.com/user-attachments/assets/aa465aef-7375-447d-8a56-b6fa6584c618" />
Link: https://editor.p5js.org/JuanSMarin2/sketches/yRdO_NoNO
