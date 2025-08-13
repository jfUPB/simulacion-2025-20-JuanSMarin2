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

