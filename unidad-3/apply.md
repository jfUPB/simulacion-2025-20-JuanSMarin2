# Unidad 3


## 🛠 Fase: Apply

## Actividad 10 El problema de los n-cuerpos

Quise inspirarme en esta obra de Alexander Calder:
<img width="1523" height="2000" alt="image" src="https://github.com/user-attachments/assets/84469348-a676-43fb-a65d-f5cae12e0012" />

Me imagino particulas que orbitan a un sol como en el ejemplo, voy a hacer que tengan formas inestables usando vertex y las lineas apuntan del centro de la particula hasta el sol. 
Para hacerlo mas interesante voy a hacer que existan multiples soles, usare la funcion lerp para interpolar los colores de las particulas en base a los colores de los soles y su distancia entre estas.


Los soles se mueven con ruido perlin como este ejemplo de la primera unidad:
<img width="639" height="238" alt="image" src="https://github.com/user-attachments/assets/7e9a865c-0e5f-4b17-80c9-fa102b2abe57" />

y tambien pegan saltos con levy flight.

y para la interacción con clic izquierdo se pueden crear soles hasta un maximo de 5 y con clic derecho se puede eliminar soles hasta un minimo de 1 para que no se escapen las particulas.


``` js
let movers = [];
let suns = [];

function setup() {
  createCanvas(800, 600);
  for (let i = 0; i < 40; i++) {
    let pos = p5.Vector.random2D();
    let vel = pos.copy();
    vel.setMag(random(3, 6));
    pos.setMag(random(200, 300));
    vel.rotate(PI / 2);
    let m = random(8, 15);
    movers[i] = new Mover(pos.x, pos.y, vel.x, vel.y, m);
  }
  suns.push(new Sun(random(10000), random(20000), 500, color(255, 0, 0)));
  background(0);
}

function draw() {
  background(0, 30);
  translate(width / 2, height / 2);
  for (let sun of suns) {
    sun.update();
    sun.show();
  }
  for (let mover of movers) {
    for (let sun of suns) {
      sun.attract(mover);
    }
    mover.update();
    mover.show(suns);
  }
  resetMatrix();
  fill(255);
  textAlign(CENTER, TOP);
  textSize(16);
  text("Click izquierdo: añadir sol (máx 5) | Click derecho: quitar sol (mín 1)\nSoles: " + suns.length, width / 2, 10);
}

function mousePressed() {
  if (mouseButton === LEFT && suns.length < 5) {
    suns.push(new Sun(random(10000), random(20000), 500, color(random(255), random(255), random(255))));
  } else if (mouseButton === RIGHT && suns.length > 1) {
    suns.pop();
  }
}

```

``` js
class Mover {
  constructor(x, y, vx, vy, m) {
    this.pos = createVector(x, y);
    this.vel = createVector(vx, vy);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * 2;
    this.t = random(1000);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distanceSq = constrain(force.magSq(), 100, 2000);
    let G = 1;
    let strength = (G * (this.mass * mover.mass)) / distanceSq;
    force.setMag(strength);
    mover.applyForce(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.set(0, 0);
    this.t += 0.01;
  }

  show(suns) {
    let d1 = this.pos.dist(suns[0].pos);
    let d2 = this.pos.dist(suns[1 % suns.length].pos);
    let t = d1 / (d1 + d2);
    let c = lerpColor(suns[0].c, suns[1 % suns.length].c, t);
    noStroke();
    fill(c);
    push();
    translate(this.pos.x, this.pos.y);
    beginShape();
    let steps = int(random(6, 10));
    for (let a = 0; a < TWO_PI; a += TWO_PI / steps) {
      let r = this.r + map(noise(this.t + cos(a), this.t + sin(a)), 0, 1, -this.r * 0.5, this.r * 0.5);
      let x = cos(a) * r;
      let y = sin(a) * r;
      vertex(x, y);
    }
    endShape(CLOSE);
    pop();
    let closestSun = suns.reduce((a, b) => this.pos.dist(a.pos) < this.pos.dist(b.pos) ? a : b);
    stroke(closestSun.c);
    strokeWeight(1.5);
    line(this.pos.x, this.pos.y, closestSun.pos.x, closestSun.pos.y);
  }
}
```

``` js
class Sun {
  constructor(tx, ty, m, c) {
    this.tx = tx;
    this.ty = ty;
    this.mass = m;
    this.r = sqrt(this.mass) * 0.7;
    this.pos = createVector(0, 0);
    this.c = c;
  }

  update() {
    if (random(1) < 0.005) {
      let angle = random(TWO_PI);
      let step = pow(random(1), -1.5) * 50;
      this.pos.add(p5.Vector.fromAngle(angle).mult(step));
    } else {
      let x = map(noise(this.tx), 0, 1, -width / 3, width / 3);
      let y = map(noise(this.ty), 0, 1, -height / 3, height / 3);
      this.pos.set(x, y);
      this.tx += 0.004;
      this.ty -= 0.004;
    }
  }

  attract(mover) {
    let force = p5.Vector.sub(this.pos, mover.pos);
    let distanceSq = constrain(force.magSq(), 100, 3000);
    let G = 2;
    let strength = (G * (this.mass * mover.mass)) / distanceSq;
    force.setMag(strength);
    mover.applyForce(force);
  }

  show() {
    noStroke();
    fill(this.c);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }
}
```

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/7abb3089-6842-43a5-88dc-6435aebba4f0" />

<img width="796" height="598" alt="image" src="https://github.com/user-attachments/assets/13e21c21-5cce-4d78-901a-bd252fece39d" />

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/406a4531-060a-47fb-9471-74315ff1b9ee" />

### Link: https://editor.p5js.org/JuanSMarin2/sketches/-aUwJY-Xs

