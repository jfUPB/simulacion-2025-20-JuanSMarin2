# Unidad 1

## 🤔 Fase: Reflect
### Actividad 09: Autoevaluación
#####
### Parte 1
#####
#### Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?
Random genera un número aleatorio entre el rango ingresado, sirve para generar una probabilidad uniforme. La función noise genera numeros dependientes del numero que ha sacado anteriormente en base al parametro ingresado para que la aletoriedad sea no uniforme y simular el ruido perlin
#### Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal? 
Una distribución de probabilidad es la forma se distribuye la probabilidad podiendo sacar todos los valores con la misma probabilidad o sesgarse a uno, una caminata aleatoria uniforme puede tomar cualquier valor e irse a cualquier dirección variando cada vez que se ejecuta, en cambio una con distribución normal se ve sesgada a una dirección o eje debido a que suele sacar los valores centrales
#### ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple
Sirve para crear variabilidad haciendo que la obra sea diferente y genere una experiencia unica cada vez que se ejecuta.
También puede usarse para reforzar una metafora o mensaje como en el ejemplo de la raices de arbol, que utiliz la aletoriedad para mostrar como la vida se forma de maneras aleatorias y como dos arboles no crecen iguales.
#### Piensa en tu obra final (Actividad 08). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas
Utilice la probabilidad no uniforme para que el walker tenga mas probabilidad de mueverse a la dirección a la que  se presione, asi el usuario puede influir en la aletoriedad del movimiento sin que este deje de ser aleatorio, fue una elección adecuada porque no podria haberse hecho con ninguno de los otros conceptos de la unidad.
#### ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?
Es un programa en el que un objeto tiene una probabilidad de moverse ciertas unidades a una de las direcciones dando la ilución de que esta desplazandose. 
La caminata Lévy flight es aquella en la que con una probabilidad mas baja puede moverse muchas mas unidades a una dirección pareciendo que hizo un salto.
#####
### Parte 2
#####
#### ¿Cuál fue el concepto más abstracto o difícil de “visualizar” para ti en esta unidad? ¿Qué hiciste para finalmente comprenderlo?
El concepto mas complejo fue naturalmente el del ruido perlin al ser el mas complejo. Tuve problemas para entender que hacia exactamente el metodo noise, que devolvia y que hacian exactamente los parametros que se le pasaban, para entenderlo tuve que ver un video y preguntarle a chat gpt para poder entenderlo.
#### Describe un momento durante el desarrollo de tu obra final (Actividad 08) en el que un “error” o un resultado inesperado te llevó a una idea creativa interesante.
Debido a los Levy Flight era muy probable que el walker se saliera lejos de la pantalla por lo que hice que al hacer click se pudiera "reiniciar" el programa borrando todo el trazo y devolviendo el walker al centro.
#### Al combinar diferentes técnicas de aleatoriedad, ¿Cuál fue el mayor desafío? ¿La interacción entre los sistemas, el control de los parámetros, el rendimiento?
Fue complejo aplicar el cambio de probabilidad en base a la direccion, tuve que buscar metodos predefinidos y varias variables booleansa para lograrlo.
#### Si tuvieras que empezar la Actividad 08 de nuevo, ¿Qué harías de manera diferente basándote en lo que sabes ahora?
Creo que llegaria al mismo resultado pero daria menos vueltas y empezaria sabiendo como aplicarlo exactamente
### Actividad 10: Coevaluación
#### Intercambien las URLs de sus bitácoras de aprendizaje: https://editor.p5js.org/JuanJAreiza/sketches/1BesSufHm

#### Basándote en la rúbrica para la actividad 08 evalúa el trabajo del compañero y escribe un comentario de retroalimentación constructiva. Esto lo harás en tu bitácora de aprendizaje.
Me gusta mucho como aplico los conceptos para generar los trazos con movimiento aleatorio pero limpio gracias al ruido perlin, tambien me parece adecuado el uso de interactividad con el cambio de paletas de colores y borrando el trazo. Lo único es que no se llega a apreciar del todo el uso la distribución Gaussiana porque casi no se alcanzan a ver los trazos de tamaño diferente.
### Actividad 11: Feedback
#### Continuar: ¿Qué actividad, ejemplo o explicación de esta unidad te resultó más reveladora o útil para tu aprendizaje? ¿Qué deberíamos mantener sí o sí?
Todos los conceptos me gustaron y me parece que todos deberian conservarse, me parece que tienen su utilidad y la curva de dificultad de aprendizaje en la manera en la que esta me parece perfecta.
El concepto de ruido perlin me parecio el mas util y el que tiene mas potencial de crear cosas muy interesantes.
#### Empezar a hacer: ¿Qué te faltó en esta unidad? ¿Quizás más ejemplos de artistas, más desafíos técnicos, más teoría? ¿Qué idea tienes para hacerla mejor?
Me parece que el concepto de ruido perlin necesita mas explicación en la propia bitacora, pienso que el libro explica bien el concepto y su utilidad pero no se explica bien la forma de aplicarlo ni como funciona bien la funcion noise.
#### Balance Teoría-Práctica: ¿Cómo sentiste el equilibrio entre analizar los ejemplos del texto guía y ponerte a programar tus propios sketches? Explica tu respuesta.
Me parecio muy adecuado y muy dinamico, me gusta como los ejemplos muestran bien a detalle como aplicar lo explicado y los conceptos no eran distantes, podian usarse para ampliar los ejemplos anteriores.

