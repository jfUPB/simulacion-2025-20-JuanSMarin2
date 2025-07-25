# Unidad 1

## 🤔 Fase: Reflect
### Actividad 09: Autoevaluación
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

