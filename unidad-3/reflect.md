# Unidad 3


## 🤔 Fase: Reflect

## Actividad 11 Autoevaluación
### Escribe la ecuación vectorial de la segunda ley de Newton y explica cada uno de sus componentes.
La suma de las fuerzas es igual a la masa por la aceleración del respectivo objeto.

### ¿Por qué es necesario multiplicar la aceleración por cero en cada frame del método update()?
Para que la aceleración no se acumule y se desborde, la aceleración necesaria se debe calcular individualmente cada frame, no es algo acumulativo.

### Explica la diferencia entre paso por valor y paso por referencia cuando aplicamos fuerzas a un objeto.
Hay que tener cuidado de que es el parametro que se pasa en un metodo:
En p5.js las variables Vector en realidad son punteros al verdadero vector por lo que al pasar un vector como parametro en un metodo se pasa la referencia al vector por lo que al modificarlo en el metodo se modifica en todo el proyecto. En cambio las otras variables si guardan directamente el valor que se le da por lo que al pasarlas en un metodo los cambios solo se aplican dentro del contexto del metodo.

### ¿Cuál es la diferencia conceptual entre modelar fuerzas (como fricción, gravedad) y simplemente definir algoritmos de aceleración?
Modelar fuerzas es aplicar conceptos fisicos en codigo para lograr un efecto deseado pero en escencia son algoritmos de aceleración.

### ¿Qué fue lo más desafiante en la Actividad 10 (problema de los n-cuerpos)? ¿El concepto creativo, la implementación de las fuerzas o la integración de la interactividad?
Lo mas desafiante fue entender como funcionaba el programa original del ejemplo para poder aplicarlo, tambien fue dificil aplicar la creacion y eliminacion de los soles en la interacción y tuve que cambiar mucho el programa para aplicarlo.

### ¿Las fuerzas que modelaste produjeron el comportamiento que esperabas? Describe un momento “sorpresa” (esperado o inesperado) durante el desarrollo.
Me sorprendio como interactuan las particulas con multiples soles que tienen movimiento, se propulsan con mucha velocidad entre los soles.

### ¿Cómo ha cambiado tu forma de pensar sobre la “física” en el arte generativo después de esta unidad?
Descubri que la fisica y los modelos no se limitan a las cosas reales si no que pueden aplicarse con las mismas formulas al codigo para crear efectos muchos mas limpios y reales.

### Si tuvieras una semana más, ¿Qué otras fuerzas te gustaría modelar o cómo mejorarías tu simulación del problema de los n-cuerpos?
Me gustaria experimentar mas con la atracción y como interactuan las diferentes fuerzas entre si.


## Actividad 12 Coevaluación
### Intercambia la URL de tu bitácora con un compañero: https://github.com/jfUPB/simulacion-2025-20-JuanJAreiza/blob/unidad3/apply/unidad-3/apply.md

### Claridad del Concepto: ¿La obra visual refleja la inspiración en las esculturas cinéticas de Calder y el problema de los n-cuerpos?
Completamente, cada captura parece una obra de Calder y las particulas se conectan y se atraen entre si.

### Implementación de Fuerzas: ¿Se aplican correctamente las leyes de Newton? ¿Las fuerzas se acumulan apropiadamente?
Si, las particulas influyen en la trayectoria de las otras, me gusta como las particulas mas grandes generan una atracción mayor.

### Creatividad en el Modelado: ¿El modelado de fuerzas es interesante y genera comportamientos únicos?
Si, me gusta como las particulas se conectan, hace que en vez de que parezcan campos gravitacionales parezca que se estan jalando entre si con una cuerda, da un comportamiento muy unico.

### Calidad de la Interactividad: ¿La interacción permite explorar diferentes aspectos del sistema de fuerzas?
Si, al resetear las conexiones se puede ver como cambia la trayectoria de las particulas y como se crean nuevas conexiones, es como si el sol dejara de atraer un planeta, este sale disparado.



## Actividad 13 Feedback
### Continuar: ¿Qué actividad o concepto de esta unidad te resultó más “revelador” para entender las fuerzas en el arte generativo?
Me sorprendio como para aplicar las fuerzas en un programa simplemente hay que aplicar las formulas que ya habiamos visto previamente en otras materias, parece intuitivo pero pense que para lograr efectos tan reales se necesitaba cosas mucho mas complejas y avanzadas.

### Dejar de hacer: ¿Hubo alguna actividad que te pareció redundante o menos efectiva para comprender el modelado de fuerzas?
Me parecio un poco redundante volver a repasar el concepto de paso por valor y referencia pero se que es un concepto importante.

### Progresión conceptual: ¿El paso de manipular aceleración directamente (unidad 2) a modelar fuerzas (unidad 3) te pareció una progresión natural y efectiva? ¿Por qué?
Si, tanto que hasta parece la misma unidad, es una progresión muy natural, siguiendo directamente donde acabó la anterior.

### Conexión arte-física: ¿Cómo te ha ayudado esta unidad a ver la conexión entre conceptos físicos y expresión artística? ¿Te sientes más cómodo “jugando” con las leyes de la física en tus creaciones?
Completamente, esto me ha ayudado a entender mejor los conceptos fisicos y se que voy a aplicarlos en mis proyectos de ahora en adelante para crear mundos mas reales.


