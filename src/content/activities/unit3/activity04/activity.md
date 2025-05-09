#### Marco Motion 101

**Entrega**: ¿Recuerdas el marco motion 101 de la unidad anterior? Regresa y dale una mirada 
rápida si es del caso. Luego considera lo siguiente:

``` js

let mover;

function setup() {
    createCanvas(640, 240);
    mover = new Mover();
}

function draw() {
    background(255);
    mover.show();
    mover.update();
    mover.checkEdges();
}

.
.
.


update() {

    // Aquí calculo la aceleración
    .
    .
    .
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
}
.
.
.

```

Mira bien el código. ¿Dónde está el marco motion 101?

- En la unidad anterior tu definías la aceleración mediante algún algoritmo. ¿Cuáles eran? Muestra 
ejemplos de código de la unidad anterior (solo la parte donde se define la aceleración).
- En esta unidad tu vas a calcular la aceleración. ¿Qué tiene que ver esto con las leyes de movimiento de Newton?

**Entrega**: texto y fragmentos de código donde des respuesta a cada una de las preguntas 
anteriores.
