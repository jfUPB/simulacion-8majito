#### Experimenta

**Enunciado**: dale una mirada a este código

``` js
let position;

function setup() {
    createCanvas(400, 400);
    posicion = createVector(6,9);
    playingVector(posicion);
    noLoop();
}

function playingVector(v){
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}

```

Para este experimento puedes usar las funciones console.log() y print() para imprimir mensajes en la consola del navegador. También puedes usar el método toString() de la clase p5.Vector para imprimir el vector en la consola.

- ¿Qué resultado esperas obtener?
- ¿Qué resultado obtuviste?
- Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en 
javascript. 
- ¿Qué tipo de paso se está realizando en el código?
- ¿Qué aprendiste?

**Entrega**: la solución a las preguntas.

