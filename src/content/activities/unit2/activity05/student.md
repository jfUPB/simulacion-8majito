function setup() {
    createCanvas(100, 100);
}

function draw() {
    background(200);
    
    //Creación vectores 
    let v0 = createVector(50, 50); // Origen de los vectores (punto de inicio)
    let v1 = createVector(30, 0); // Vector rojo (horizontal)
    let v2 = createVector(0, 30); // Vector azul (vertical)
    
    //Interpolación Lineal con lerp()
    let v3 = p5.Vector.lerp(v1, v2, 0.5); // Crea v3, un vector intermedio entre v1 y v2
    // Toma un punto a mitad de camino entre v1 y v2.
    
    //Dibuja los vectores
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v3, 'purple');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor); // Color de la flecha
    strokeWeight(3); // Grosor de la línea
    fill(myColor);
    
    translate(base.x, base.y); // Mueve el origen de coordenadas a `base`
    
    line(0, 0, vec.x, vec.y); // Dibuja la línea desde (0,0) hasta (vec.x, vec.y)
    
    rotate(vec.heading()); // Rota en la dirección del vector

    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0); // Mueve el punto de la flecha
    
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja la punta de la flecha
    
    pop();
}


#### El código que genera el resultado propuesto.
- Agrande el canvas a 400x400
- Alargue los vectores
- Calculé un vector verde: Utilicé *p5.Vector.sub(v2, v1)* para calcular la diferencia entre los vectores v1 y v2, lo que da el vector verde que conecta estos dos puntos.
- Usé p5.Vector.add(v0, v1) para mover el punto de origen de la flecha verde correctamente, de modo que esta se conecte a las otras flechas.
- Dibujo la flecha
- Añadí suavizado de movimiento: En lugar de incrementar t con un valor constante (t += 0.01),use lerp() para que fuera mas fluida y controlada para suavizar el movimiento.
- Usé interpolación de colores: lerpColor(), podrías cambiar el color de la flecha morada conforme se mueve de v1 a v2 para que la flecha se "despinte" o "cambie de color" durante su movimiento.
Ciclo o rebote de la animación:

En lugar de simplemente reiniciar la animación cuando t llega a 1, podrías hacer que la animación rebote de vuelta, creando un movimiento de ida y vuelta.
  
function setup() {
    createCanvas(400, 400);
}

let t = 0;  // Controlador de la interpolación
let direction = 1; // Dirección de la animación (1 = avanzando, -1 = retrocediendo)

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(200);

    let v0 = createVector(30, 50);
    let v1 = createVector(300, 0);
    let v2 = createVector(0, 300);

    // Interpolación de la flecha morada entre v1 y v2
    let v3 = p5.Vector.lerp(v1, v2, t);
  
    // Calcula el vector entre v1 y v2 para la flecha verde
    let vGreen = p5.Vector.sub(v2, v1);

    // Dibuja las flechas
    drawArrow(v0, v1, 'red');  // Flecha roja de v0 a v1
    drawArrow(v0, v2, 'blue'); // Flecha azul de v0 a v2
    drawArrow(v0, v3, 'purple'); // Flecha morada que se mueve
    drawArrow(p5.Vector.add(v0, v1), vGreen, 'green'); // Flecha verde de v1 a v2

    // Actualiza t, el valor de interpolación, para mover la flecha
    t += 0.01 * direction;

    // Cambia la dirección cuando t llegue a 0 o 1
    if (t >= 1 || t <= 0) {
        direction *= -1;  // Cambia la dirección del movimiento
    }
}

// Función para dibujar las flechas
function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}



#### ¿Cómo funciona lerp() y lerpColor().
#### ¿Cómo se dibuja una flecha usando drawArrow()?
