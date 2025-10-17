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

## Incluye el código completo de tu sketch final.
``` js
const { Engine, World, Bodies, Body, Constraint, Events, Vector } = Matter;

let engine, world;
let floor;
let B, eLetter;
let leftT, rightT;
let shield, sword;
let state = "idle", st = 0;
let mode = "battle";
let transition = null;
let sparks = [];
let baseY, xB, xA, xT1, xT2, xL, xE, GAP = 88;
let slash;

function preload(){
  soundFormats('wav','mp3');
  slash = loadSound('slash.wav');
}

function setup(){
  createCanvas(920, 560);
  textFont("Arial Black"); textAlign(CENTER,CENTER);
  document.oncontextmenu = e => e.preventDefault();
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0;
  baseY = height*0.58;
  xB = width*0.18;
  xA = xB + GAP;
  xT1 = xA + GAP;
  xT2 = xT1 + GAP;
  xL = xT2 + GAP;
  xE = xL + GAP;
  B = makeLetter('B', xB, baseY, 58,68);
  eLetter = makeLetter('e', xE, baseY, 50,54);
  leftT  = new TChar(xT1, baseY, 'right', -1);
  rightT = new TChar(xT2, baseY, 'left',  -2);
  shield = makeWeapon('a', leftT.handPoint('L').x, leftT.handPoint('L').y, 52,56, leftT.group);
  sword  = makeWeapon('L', rightT.handPoint('R').x, rightT.handPoint('R').y, 52,66, rightT.group);
  attachWeapon(shield, leftT,  'L');
  attachWeapon(sword,  rightT, 'R');
  floor = Bodies.rectangle(width/2, height*0.74, width, 20, {isStatic:true});
  World.add(world, floor);
  Events.on(engine,"collisionStart", ev=>{
    for(const p of ev.pairs){
      const A=p.bodyA.label, B=p.bodyB.label;
      if ((A==='L'&&B==='a')||(A==='a'&&B==='L')){
        const pt = p.collision.supports[0];
        spawnSpark(pt.x, pt.y);
        Body.applyForce(leftT.torso,  leftT.torso.position,  {x:-0.02,y:-0.005});
        Body.applyForce(rightT.torso, rightT.torso.position, {x: 0.02,y:-0.005});
      }
    }
  });
}

function draw(){
  background(170);
  Engine.update(engine, 1000/60);
  if (transition && transition.active) updateTransition();
  else if (mode === "battle") choreography();
  renderBody(B);
  renderBody(eLetter);
  drawT(leftT);
  drawT(rightT);
  renderBody(shield);
  renderBody(sword);
  drawSparks();
}

function keyPressed(){
  if (key === 'f' || key === 'F'){
    const fs = fullscreen();
    fullscreen(!fs);
    setTimeout(()=>windowResized(), 50);
  }
}

function windowResized(){
  resizeCanvas(windowWidth, windowHeight);
  baseY = height*0.58;
  xB = width*0.18;
  xA = xB + GAP;
  xT1 = xA + GAP;
  xT2 = xT1 + GAP;
  xL = xT2 + GAP;
  xE = xL + GAP;
  Body.setPosition(B, {x:xB, y:baseY});
  Body.setPosition(eLetter, {x:xE, y:baseY});
  Body.setPosition(floor, {x:width/2, y:height*0.74});
}

function pill(g, ch, x, y, w, h, dark=false, s=72){
  g.push(); g.translate(x,y); g.rectMode(CENTER);
  g.noStroke(); g.fill(0,50); g.rect(6,6,w,h,10);
  g.stroke(dark?65:190); g.strokeWeight(6); g.fill(dark?25:240); g.rect(0,0,w,h,10);
  g.noStroke(); g.fill(dark?240:40); g.textAlign(CENTER,CENTER); g.textSize(s);
  g.text(ch,0,6); g.pop();
}
function renderBody(b){
  const { ch, w, h, isWeapon } = b._render;
  push(); translate(b.position.x, b.position.y); rotate(b.angle);
  if (isWeapon) {
    noStroke(); fill(255); textAlign(CENTER,CENTER); textSize(72);
    text(ch, 0, 6);
  } else {
    pill(this, ch, 0,0,w,h, false, 72);
  }
  pop();
}
function drawT(t){
  function seg(x,y,w,h,a){
    push(); translate(x,y); rotate(a);
    noStroke(); fill(0,50); rect(6,6,w,h,8);
    stroke(65); strokeWeight(6); fill(25); rect(0,0,w,h,8);
    pop();
  }
  seg(t.torso.position.x, t.torso.position.y, 26,86, t.torso.angle);
  seg(t.armL.position.x, t.armL.position.y, 46,14, t.armL.angle);
  seg(t.armR.position.x, t.armR.position.y, 46,14, t.armR.angle);
}

class TChar {
  constructor(x, y, face, group) {
    this.face = face;
    this.group = group;
    this.torso = Bodies.rectangle(x, y, 26, 86, phys(group, 0.012));
    this.armL  = Bodies.rectangle(x-30, y-6, 46, 14, phys(group, 0.006));
    this.armR  = Bodies.rectangle(x+30, y-6, 46, 14, phys(group, 0.006));
    this.jointL = Constraint.create({ bodyA:this.torso, pointA:{x:-12,y:-20}, bodyB:this.armL, pointB:{x:-18,y:0}, length:12, stiffness:.9, damping:.2 });
    this.jointR = Constraint.create({ bodyA:this.torso, pointA:{x: 12,y:-20}, bodyB:this.armR, pointB:{x: 18,y:0}, length:12, stiffness:.9, damping:.2 });
    World.add(world, [this.torso,this.armL,this.armR,this.jointL,this.jointR]);
  }
  slerp(body, target, k=.25){
    let a = body.angle, d = target - a;
    while (d >  Math.PI) d -= TWO_PI;
    while (d < -Math.PI) d += TWO_PI;
    Body.setAngle(body, a + d*k);
  }
  pose(torsoA, armLA, armRA){
    this.slerp(this.torso, torsoA);
    this.slerp(this.armL,  armLA);
    this.slerp(this.armR,  armRA);
  }
  handPoint(side){
    const b = side==='L' ? this.armL : this.armR;
    const local = side==='L' ? Vector.create(18,0) : Vector.create(-18,0);
    const cs = Math.cos(b.angle), sn = Math.sin(b.angle);
    const off = Vector.create(local.x*cs - local.y*sn, local.x*sn + local.y*cs);
    return Vector.add(b.position, off);
  }
}

function phys(group, density=0.001){
  return { frictionAir:.06, restitution:.2, density, collisionFilter:{ group } };
}
function makeLetter(ch,x,y,w,h){
  const body = Bodies.rectangle(x,y,w,h,{ isStatic:true });
  body._render = { ch, w, h, isWeapon:false };
  World.add(world, body); return body;
}
function makeWeapon(ch,x,y,w,h,group){
  const body = Bodies.rectangle(x,y,w,h, phys(group, 0.006));
  body.label = ch;
  body._render = { ch, w, h, isWeapon:true };
  World.add(world, body); return body;
}
function attachWeapon(weapon, tchar, side){
  if (weapon._link) World.remove(world, weapon._link);
  const arm = side==='L' ? tchar.armL : tchar.armR;
  const ptA = side==='L' ? {x:-8,y:-6} : {x: 8,y:-6};
  const ptB = side==='L' ? {x: 14,y: 0} : {x:-14, y: 0};
  weapon._link = Constraint.create({ bodyA: arm, pointA: ptB, bodyB: weapon, pointB: ptA, length: 0, stiffness:.9, damping:.18 });
  World.add(world, weapon._link);
  weapon._onSide = side;
}

function choreography(){
  const dt = 1/60; st += dt;
  const ease = t => t<.5 ? 4*t*t*t : 1 - Math.pow(-2*t+2,3)/2;
  function idlePose(T, side){
    const s = side==='left' ? -1 : 1;
    T.pose( 0.06*s, -0.15*s, 0.10*s );
  }
  function windupPose(T, side, t){
    const s = side==='left' ? -1 : 1, k=ease(t);
    T.pose( lerp(0.06*s, 0.25*s, k), lerp(-0.15*s, -0.65*s, k), lerp( 0.10*s, -0.10*s, k));
  }
  function strikePose(T, side, t){
    const s = side==='left' ? -1 : 1, k=ease(t);
    T.pose( lerp(0.25*s, -0.05*s, k), lerp(-0.65*s,  0.05*s, k), lerp(-0.10*s,  0.25*s, k));
  }
  function recoilPose(T, side, t){
    const s = side==='left' ? -1 : 1, k=ease(t);
    T.pose( lerp(-0.05*s, 0.08*s, k), lerp( 0.05*s,-0.25*s, k), lerp( 0.25*s, 0.05*s, k));
  }
  if (state==="idle"){
    idlePose(leftT,'left'); idlePose(rightT,'right');
  } else if (state==="windup"){
    const t = constrain(st/.5,0,1);
    windupPose(rightT,'right',t); windupPose(leftT,'left',t);
    if (t>=1){
      if (shield._onSide!=='R') attachWeapon(shield, leftT,  'R');
      if (sword._onSide!=='L')  attachWeapon(sword,  rightT, 'L');
      st=0; state="strike";
    }
  } else if (state==="strike"){
    const t = constrain(st/.35,0,1);
    strikePose(rightT,'right',t); strikePose(leftT,'left',t);
    pullTogether(sword, shield, 0.0010);
    if (t>=1){ st=0; state="recoil"; }
  } else if (state==="recoil"){
    const t = constrain(st/.45,0,1);
    recoilPose(rightT,'right',t); recoilPose(leftT,'left',t);
    if (t>=1){
      attachWeapon(shield, leftT,'L');
      attachWeapon(sword,  rightT,'R');
      st=0; state="settle";
    }
  } else if (state==="settle"){
    const t = constrain(st/.4,0,1);
    idlePose(leftT,'left'); idlePose(rightT,'right');
    if (t>=1){ state="idle"; st=0; }
  }
}

const easeInOut = t => t<.5 ? 4*t*t*t : 1 - Math.pow(-2*t+2,3)/2;

function startTransition(toMode){
  state = "idle"; st = 0;
  transition = { active:true, t:0, dur:0.6, items:[] };
  if (toMode === 'assembled') {
    if (shield._link){ World.remove(world, shield._link); shield._link=null; }
    if (sword._link){  World.remove(world, sword._link);  sword._link=null; }
  }
  const bodies = [leftT.torso,leftT.armL,leftT.armR,rightT.torso,rightT.armL,rightT.armR,shield,sword];
  const targets = {};
  if (toMode === 'assembled') {
    targets[leftT.torso  .id] = {x:xT1, y:baseY,   a:0};
    targets[leftT.armL   .id] = {x:xT1-30, y:baseY-6, a:0};
    targets[leftT.armR   .id] = {x:xT1+30, y:baseY-6, a:0};
    targets[rightT.torso .id] = {x:xT2, y:baseY,   a:0};
    targets[rightT.armL  .id] = {x:xT2-30, y:baseY-6, a:0};
    targets[rightT.armR  .id] = {x:xT2+30, y:baseY-6, a:0};
    targets[shield.id]         = {x:xA, y:baseY, a:0};
    targets[sword.id]          = {x:xL, y:baseY, a:0};
  } else {
    targets[leftT.torso  .id] = {x:xT1, y:baseY,   a:0};
    targets[leftT.armL   .id] = {x:xT1-30, y:baseY-6, a:-0.15};
    targets[leftT.armR   .id] = {x:xT1+30, y:baseY-6, a: 0.10};
    targets[rightT.torso .id] = {x:xT2, y:baseY,   a:0};
    targets[rightT.armL  .id] = {x:xT2-30, y:baseY-6, a:-0.10};
    targets[rightT.armR  .id] = {x:xT2+30, y:baseY-6, a: 0.15};
    targets[shield.id]         = {x:leftT.handPoint('L').x,  y:leftT.handPoint('L').y,  a:0};
    targets[sword.id]          = {x:rightT.handPoint('R').x, y:rightT.handPoint('R').y, a:0};
  }
  for (const b of bodies) {
    transition.items.push({ body: b, from: { x:b.position.x, y:b.position.y, a:b.angle }, to: targets[b.id] });
    Body.setVelocity(b, {x:0,y:0});
    Body.setAngularVelocity(b, 0);
  }
  transition.toMode = toMode;
}

function updateTransition(){
  transition.t += deltaTime/1000;
  const k = easeInOut(constrain(transition.t/transition.dur, 0, 1));
  for (const it of transition.items) {
    const x = lerp(it.from.x, it.to.x, k);
    const y = lerp(it.from.y, it.to.y, k);
    let da = it.to.a - it.from.a;
    while (da >  Math.PI) da -= TWO_PI;
    while (da < -Math.PI) da += TWO_PI;
    const a = it.from.a + da * k;
    Body.setPosition(it.body, {x,y});
    Body.setAngle(it.body, a);
    Body.setVelocity(it.body, {x:0,y:0});
    Body.setAngularVelocity(it.body, 0);
  }
  if (k >= 1) {
    transition.active = false;
    mode = transition.toMode;
    if (mode === 'battle') {
      attachWeapon(shield, leftT,'L');
      attachWeapon(sword,  rightT,'R');
      state = "idle"; st = 0;
    }
  }
}

function mousePressed(){
  if (mouseButton === RIGHT) {
    if (!transition || !transition.active) startTransition('assembled');
    return;
  }
  if (!transition || !transition.active) {
    startTransition('battle');
    setTimeout(()=>{ if (mode==='battle' && (!transition||!transition.active)) { state="windup"; st=0; } }, 650);
  }
}

function pullTogether(A,B,k){
  const dir = Vector.sub(B.position, A.position);
  const d = Vector.magnitude(dir)+1e-6;
  const f = Vector.mult(Vector.normalise(dir), k*d);
  Body.applyForce(A, A.position, f);
  Body.applyForce(B, B.position, Vector.neg(f));
}

function spawnSpark(x,y){
  sparks.push({x,y,r:6,life:20,rot:random(TWO_PI)});
  if (slash && slash.isLoaded()) { slash.stop(); slash.play(); }
}
function drawSparks(){
  for (const s of sparks){
    s.r+=5; s.life--;
    push(); translate(s.x,s.y); rotate(s.rot);
    noStroke(); fill(255,208,64, map(s.life,0,20,0,220));
    star(0,0,s.r*0.35,s.r,7); pop();
  }
  sparks = sparks.filter(s=>s.life>0);
}
function star(x,y,r1,r2,n){
  beginShape();
  for(let i=0;i<n*2;i++){
    const a=(PI/n)*i;
    const r=(i%2? r1:r2);
    vertex(x+cos(a)*r, y+sin(a)*r);
  }
  endShape(CLOSE);
}

function makeLetter(ch,x,y,w,h){
  const body = Bodies.rectangle(x,y,w,h,{ isStatic:true });
  body._render = { ch, w, h, isWeapon:false };
  World.add(world, body); return body;
}
function makeWeapon(ch,x,y,w,h,group){
  const body = Bodies.rectangle(x,y,w,h, phys(group, 0.006));
  body.label = ch;
  body._render = { ch, w, h, isWeapon:true };
  World.add(world, body); return body;
}
function phys(group, density=0.001){
  return { frictionAir:.06, restitution:.2, density, collisionFilter:{ group } };
}

```

<img width="670" height="290" alt="image" src="https://github.com/user-attachments/assets/08cf4e6e-e8be-4337-a2fe-d1807b832bc6" />

<img width="636" height="398" alt="image" src="https://github.com/user-attachments/assets/e884d6a3-7d72-4c60-984b-7cb6acc8c5b9" />


![20251016-0409-51 7109228](https://github.com/user-attachments/assets/2023e44b-f4a4-495c-b0e1-f7ef242e300e)


### Link: https://editor.p5js.org/JuanSMarin2/sketches/On_m4WEAD


## AutoEvaluación

### Actividad 1
Nota: 5.0: Analicé ejemplos con detalle y expresé mis propias ideas con comprensión y originalidad, especialmente en Battle, que use luego como la base para la animación.

### Actividad 2
Nota: 5.0: Realicé correctamente ambos experimentos. Entendí y expliqué los conceptos principales con mis propias palabras.

### Actividad 3: Apply
Nota: 5.0: Implementé un sistema complejo con físicas, constraints, animación por fases, colisiones y efectos visuales. La pieza tiene coherencia conceptual y técnica, logrando unir tipografía y física expresiva.
