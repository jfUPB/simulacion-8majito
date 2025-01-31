- funciones console.log() y print() para imprimir mensajes en la consola del navegador. 
-  método toString() de la clase p5.Vector para imprimir el vector en la consola.

#### Código 

let position;

function setup() {
    createCanvas(400, 400);  // Crea un lienzo de 400x400 píxeles
    posicion = createVector(6, 9);  // Inicializa el vector con las coordenadas (6, 9)
    playingVector(posicion);  // Modifica las componentes del vector
    noLoop();  // Detiene el loop de draw(), solo se ejecuta una vez
}

function playingVector(v) {
    v.x = 20;  // Modifica la componente x del vector a 20
    v.y = 30;  // Modifica la componente y del vector a 30
}

function draw() {
    background(220);  // Establece un fondo gris
    console.log("Only once");  // Imprime "Only once" en la consola
    // Si quieres ver el cambio visualmente, puedes agregar esto:
    // ellipse(posicion.x, posicion.y, 10, 10);  // Dibuja un círculo en (20, 30)
}

#### ¿Qué resultado esperas obtener?

#### ¿Qué resultado obtuviste?

#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.

#### ¿Qué tipo de paso se está realizando en el código?

#### ¿Qué aprendiste
