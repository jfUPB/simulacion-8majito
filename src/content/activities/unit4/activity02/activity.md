#### Conceptos fundamentales

**Enunciado**: analiza las siguientes simulaciones y responde las preguntas.

Te voy a proponer un par de simulaciones para que analices.

Primero mira esta simulación para el [manejo de ángulos](https://editor.p5js.org/juanferfranco/sketches/R1iTVQjzm).

- ¿Qué está pasando en esta simulación? ¿Cuál es la interacción? 
- Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?
- Cuál es la relación entre el sistema de coordenadas y la función `rotate()`.

Nota esta parte del código:

``` javascript
  line(-50, 0, 50, 0);
  stroke(0);
  strokeWeight(2);
  fill(127);
  circle(50, 0, 16);
  circle(-50, 0, 16);
```

Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición `(0, 0)` del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

Ahora analiza una simulación que muestra cómo puedes hacer para que los elementos gráficos de la simulación [apunten en la dirección del movimiento](https://editor.p5js.org/natureofcode/sketches/bZqHGYbRQ).

- Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

Observa detenidamente este fragmento de código de la simulación:

``` js
  display() {
    let angle = this.velocity.heading();

    stroke(0);
    strokeWeight(2);
    fill(127);
    push();
    rectMode(CENTER);
    translate(this.position.x, this.position.y);
    rotate(angle);
    rect(0, 0, 30, 10);

    pop();
  }
```

- ¿Qué hace la función `heading()`?
- ¿Qué hace la función `push()` y `pop()`? Realiza algunos experimentos para entender su funcionamiento.
- ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.
- ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel 
el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.

**Entrega**: reporta la respuesta a las preguntas anteriores.

