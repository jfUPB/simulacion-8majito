### Codigo de analisis:
```js
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
```

#### El código que genera el resultado propuesto.
- Agrande el canvas a 400x400
- Alargue los vectores
- Calculé un vector verde: Utilicé *p5.Vector.sub(v2, v1)* para calcular la diferencia entre los vectores v1 y v2, lo que da el vector verde que conecta estos dos puntos.
- Usé p5.Vector.add(v0, v1) para mover el punto de origen de la flecha verde correctamente, de modo que esta se conecte a las otras flechas.
- Dibujo la flecha
- Añadí suavizado de movimiento: En lugar de incrementar t con un valor constante (t += 0.01),use lerp() para que fuera mas fluida y controlada para suavizar el movimiento.
- Rebote de la animación: Implementamos un rebote de la flecha morada utilizando la variable t para controlar su interpolación, y cambiamos la dirección de la animación cuando alcanza los valores 0 o 1.
-Interapolarmos colores con Lerpcolor: 
```js
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
  
    // Calcula la interpolación entre v1 y v2 con t variando de 0 a 0.99
    let v3 = p5.Vector.lerp(v1, v2, t);
  
    // Calcula los vectores entre v1 y v2 (verde)
    let vGreen = p5.Vector.sub(v2, v1);
  
    // Colores a interpolar
    let redColor = color(255, 0, 0); // Rojo
    let purpleColor = color(128, 0, 128); // Morado
    let blueColor = color(0, 0, 255); // Azul

    // Interpolación de colores
    let currentColor;
    if (t < 0.5) {
        // De rojo a morado
        currentColor = lerpColor(redColor, purpleColor, t * 2); // Multiplicamos por 2 para que el cambio ocurra más rápido en la primera mitad
    } else {
        // De morado a azul
        currentColor = lerpColor(purpleColor, blueColor, (t - 0.5) * 2); // Multiplicamos por 2 para que el cambio ocurra más rápido en la segunda mitad
    }

    // Dibuja las flechas
    drawArrow(v0, v1, 'red');   // Flecha roja de v0 a v1
    drawArrow(v0, v2, 'blue');  // Flecha azul de v0 a v2
    drawArrow(v0, v3, currentColor); // Flecha morada con color interpolado
    drawArrow(p5.Vector.add(v0, v1), vGreen, 'green'); // Flecha verde de v1 a v2

      // Actualiza t, el valor de interpolación, para mover la flecha
    t += 0.01 * direction;

    // Cambia la dirección cuando t llegue a 0 o 1
    if (t >= 1 || t <= 0) {
        direction *= -1;  
    }
}

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
```

![image](../../../../../assets/Grabación 2025-02-05 112928.gif)
#### ¿Cómo funciona lerp() y lerpColor().

**lerp()**

- Es una función de interpolación lineal que se utiliza para calcular un valor intermedio entre dos números. Se expresa como lerp(start, stop, amt), donde:
- start: El valor de inicio (el valor desde el cual se empieza).
- stop: El valor final (el valor al cual se quiere llegar).
- amt: Un número entre 0 y 1 que representa la cantidad de interpolación entre el valor de inicio y el valor final. Si amt = 0, el valor es igual a start; si amt = 1, el valor es igual a stop; valores intermedios producen valores interpolados.
- 
En el contexto de los vectores, como hicimos en el código, lerp() calcula un punto intermedio entre dos vectores. P5.Vector.lerp(v1, v2, t) nos da un vector en la línea que conecta v1 y v2, dependiendo de la proporción de t. Cuando t varía entre 0 y 1, el vector se mueve gradualmente de v1 a v2.

**lerpColor():**

lerpColor() es similar a lerp(), pero en lugar de trabajar con números, trabaja con colores. La función se usa de la forma lerpColor(color1, color2, amt), donde:

- color1: El primer color de inicio.
- color2: El color final al que quieres llegar.
- amt: La cantidad de interpolación, que también varía entre 0 y 1. Cuando amt = 0, el color será igual a color1, y cuando amt = 1, el color será igual a color2. Los valores intermedios crean un gradiente suave entre los dos colores.
  
En el código, usé lerpColor() para cambiar el color de la flecha morada, pasando de rojo a morado y luego de morado a azul, dependiendo del valor de t.

#### ¿Cómo se dibuja una flecha usando drawArrow()?

**drawArrow():**

Es una función personalizada que dibuja una flecha en el lienzo. Se usa de la siguiente forma: drawArrow(base, vec, myColor), donde:

- base: El punto de inicio de la flecha (un vector). Es el punto desde donde la flecha comienza a dibujarse.
- vec: El vector que define la dirección y magnitud de la flecha. Este vector determina hacia dónde apunta la flecha y cuánto mide.
- myColor: El color de la flecha. Se pasa como un valor de color (por ejemplo, color(255, 0, 0) para rojo).
- push() y pop(): Se usan para guardar y restaurar el estado de las transformaciones, como la posición y rotación, para que no afecten a otros dibujos.
- translate(base.x, base.y): Desplaza el origen del sistema de coordenadas a las coordenadas del vector base.
- line(0, 0, vec.x, vec.y): Dibuja una línea desde el origen (0, 0) hasta las coordenadas del vector vec que define la dirección y magnitud de la flecha.
- rotate(vec.heading()): Rota el sistema de coordenadas para que la línea se dibuje en la dirección correcta.
- triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0): Dibuja el triángulo de la punta de la flecha, ubicado en la dirección final del vector, para representar la punta de la flecha.
  
Esto permite crear flechas que se dibujan con el color y la orientación que definimos, adaptándose a las coordenadas de los vectores que se pasan como argumentos.
