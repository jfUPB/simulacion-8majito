modifica de nuevo el programa de la actividad anterior, pero esta vez cambia la base de las flechas y la escala de los vectores usando el mouse.

#### Código con la modificación.

```js
let t = 0;  // Controlador de la interpolación
let direction = 1; // Dirección de la animación (1 = avanzando, -1 = retrocediendo)

function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(200);

    // Base completamente dentro del canvas
    let baseX = constrain(mouseX, 0, width);
    let baseY = constrain(mouseY, 0, height);
    let base = createVector(baseX, baseY);

    // Factor de escala basado en la posición del mouse
    let scaleFactor = constrain(map(mouseX, 0, width, 0.5, 1.5), 0.5, 1.5);

    // Vectores escalados
    let v1 = createVector(200, 0).mult(scaleFactor);
    let v2 = createVector(0, 200).mult(scaleFactor);

    // Asegurar que los vectores no salgan del canvas
    v1.limit(width - baseX);
    v2.limit(height - baseY);
  
    // Vector interpolado entre v1 y v2
    let v3 = p5.Vector.lerp(v1, v2, t);

    // Vector verde entre v1 y v2
    let vGreen = p5.Vector.sub(v2, v1);

    // Colores a interpolar
    let redColor = color(255, 0, 0);
    let purpleColor = color(128, 0, 128);
    let blueColor = color(0, 0, 255);

    // Interpolación de colores
    let currentColor;
    if (t < 0.5) {
        currentColor = lerpColor(redColor, purpleColor, t * 2);
    } else {
        currentColor = lerpColor(purpleColor, blueColor, (t - 0.5) * 2);
    }

    // Dibujar flechas
    drawArrow(base, v1, 'red');
    drawArrow(base, v2, 'blue');
    drawArrow(base, v3, currentColor);
    drawArrow(p5.Vector.add(base, v1), vGreen, 'green');

    // Actualizar t para la animación
    t += 0.01 * direction;

    // Cambiar la dirección cuando t llegue a 0 o 1
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
src/assets/Act6U2.gif

#### Explica cómo solucionaste el problema
