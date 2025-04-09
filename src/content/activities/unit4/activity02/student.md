#### ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

En esta simulación, se dibuja una linea con dos círculos en sus extremos. El centro de la linea está en el centro de la pantalla. La interacción ocurre cuando se presiona una tecla, lo que hace que el ángulo de rotación aumente, girando la figura alrededor de su centro.

#### Se traslada el origen al centro de la pantalla para que la rotación ocurra alrededor de ese punto

Se traslada el origen al centro de la pantalla para que la rotación ocurra alrededor de ese punto.

#### ¿Cuál es la relación entre el sistema de coordenadas y la función rotate()?

La función rotate(angle) gira el sistema de coordenadas en torno a su origen actual. Como en la simulación se usa translate(width / 2, height / 2), el origen se mueve al centro de la pantalla, y la rotación ocurre en torno a ese punto. Si translate() no estuviera, la rotación se realizaría en la esquina superior izquierda de la pantalla (0,0), lo que generaría un movimiento diferente.

#### ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Se dibujan los elementos con coordenadas relativas a (0,0) porque el sistema de coordenadas se traslada al centro de la pantalla con translate(width / 2, height / 2), haciendo que todo se posicione respecto a ese punto.

Los elementos rotan porque rotate(angle) gira todo el sistema de coordenadas antes de dibujar. Como angle aumenta al presionar una tecla, el sistema se inclina más en cada frame, haciendo que las figuras parezcan girar alrededor del centro. Sin rotate(), los elementos quedarían fijos en su orientación.

#### Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

```js
this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
```

En este código, un objeto se mueve en dirección al mouse aplicando una aceleración que lo empuja hacia él. La velocidad se actualiza en cada frame y se limita a un valor máximo (topspeed), lo que simula un movimiento más natural y progresivo. Además, el objeto reaparece en el lado opuesto de la pantalla cuando llega a los bordes, creando un efecto de movimiento continuo.

Análisis del código en display()

#### ¿Qué hace la función heading()?
La función heading() obtiene el ángulo de dirección del vector de velocidad en radianes. Esto es útil para alinear visualmente el objeto en la misma dirección en la que se mueve.

#### ¿Qué hacen push() y pop()?

push() guarda el estado actual de transformación del sistema de coordenadas (posición, rotación, escalado, etc.).
pop() restaura el estado guardado antes de la transformación, evitando que afecte otros elementos en el lienzo.
Esto es importante porque translate() y rotate() modifican el sistema de coordenadas, y sin push() y pop(), afectarían todo lo que se dibuje después.

##### ¿Qué hace rectMode(CENTER)?

Esta función cambia el punto de referencia de los rectángulos. En modo CENTER, el rectángulo se dibuja desde su centro en lugar de su esquina superior izquierda. Esto facilita alinearlo con la rotación del objeto.

##### Relación entre el ángulo de rotación y el vector de velocidad

El ángulo de rotación (angle) se obtiene de la dirección del vector de velocidad (velocity.heading()). Como rotate(angle) gira el sistema de coordenadas, el rectángulo se orienta en la dirección en la que se mueve el objeto.
