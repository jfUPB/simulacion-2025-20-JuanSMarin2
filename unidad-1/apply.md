# Unidad 1

## 🛠 Fase: Apply

### Actividad 08: Creación de obra generativa
### Concepto inicial: Idea antes de empezar a programar
Un trazo de circulos con movimiento con caminata aleatoria, cada cierto tiempo pega un salto a una de las 8 direcciones. Con las flechas se puede alterar la probabilidad favoreciendo la direccion presionada cambiando el movimiento de uniforme a no uniforme

``` js

// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}


function keyPressed() {
  
  if(keyCode === LEFT_ARROW){
     walker.isLeft = true;
  } 
    if(keyCode === RIGHT_ARROW){
     walker.isRight = true;
  } 
    if(keyCode === UP_ARROW){
     walker.isUp = true;
  } 
    if(keyCode === DOWN_ARROW){
     walker.isDown = true;
  } 
  
  
  
}


function keyReleased() {

  
  walker.clearKeys();

}

function mouseClicked() {
   background(255);
   walker.clearStroke();
  
}

class Walker {
  

  
  
  constructor() { 
    
    this.clearKeys();
    this.x = width / 2;
    this.y = height / 2;

  }
  
  clearKeys(){
    this.isLeft = false;
    this.isRight = false;
    this.isUp = false;
    this.isDown = false;

    
  }
  clearStroke(){
    this.x = width/2;
    this.y = height/2;
  }

  show() {
    
    this.noiseOffset = random(1000);
    
    let r = noise(this.noiseOffset) * 255;
  let g = noise(this.noiseOffset + 100) * 255;
  let b = noise(this.noiseOffset + 200) * 255;

  stroke(r, g, b);
    
    
    strokeWeight(5)
    circle(this.x, this.y, 10);
    
  }

  step() {
    const choice = floor(random(9));
    const jump = random(1);
  
    let amount;
  
    amount = jump > 0.01? 1 : 100;  
    
    
    if(!this.isRight && !this.isLeft && !this.isUp && !this.isDown){ //Si no se presiona alguna tecla
      
        if (choice == 0 ) {
      this.x += amount;
    } else if (choice == 1) {
      this.x -= amount;
    } else if (choice == 2) {
      this.y += amount;
    } else if (choice == 3) {
      this.y -= amount;
    }
  } else { // Si se preciona una tecla
    
    if(this.isRight && (choice == 0 || choice == 1 || choice == 2 || choice == 3)){
      this.x += amount;
    } else if (this.isLeft && (choice == 0 || choice == 1 || choice == 2 || choice == 3)){ //else if porque son incompatibles
      this.x -= amount;
    } 
    
    
    if (this.isUp && (choice == 4 || choice == 5 || choice == 6 || choice == 7)){
      this.y -= amount;
    } else if (this.isDown && (choice == 4 || choice == 5 || choice == 6 || choice == 7)){ //else if porque son incompatibles
               this.y += amount;
               }
    
    
    
    
    
  }
      
      
      
    }
    
    

}





```

### Concepto final: Descripción despues de terminar
se genera en tiempo un trazo de círculos con una caminata aleatoria. El color de los círculos cambia suavemente usando la función noise() con ruido perlin para los valores RGB. Se puede interactuar usando las teclas de flechas para influir en la dirección del movimiento, cambiando la probabilidad de desplazamiento y cambinado la distribución de uniforme a no uniforme. Al hacer clic con el mouse, la pantalla se limpia y el trazo comienza de nuevo desde el centro.

#####

<img width="679" height="282" alt="image" src="https://github.com/user-attachments/assets/671b188c-bbc6-448f-87c8-bf286a438f2b" />
<img width="598" height="238" alt="image" src="https://github.com/user-attachments/assets/0ebb7d79-9703-4d37-a2d2-71e15e062322" />

#### Enlace: https://editor.p5js.org/JuanSMarin2/sketches/KRXUBuzGr


