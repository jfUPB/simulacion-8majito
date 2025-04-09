Para hacer que la onda se mueva como una ola, actualizace el dibujo en cada fotograma en draw() en lugar de dibujar todo en setup(). Esto permitirá que la onda fluya y se vea animada.

```js
// Variables para controlar la onda
let angle = 0;
let angleVelocity = 0.2;
let amplitude = 100;
let xSpacing = 24; // Espaciado entre los puntos de la onda
let offset = 0; // Desplazamiento para que la onda se mueva

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);
  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  let currentAngle = angle; // Reiniciamos el ángulo para cada onda

  for (let x = 0; x <= width; x += xSpacing) {
    let y = amplitude * sin(currentAngle + offset); // Calcula la posición y
    circle(x, y + height / 2, 48); // Dibuja el círculo en la onda
    currentAngle += angleVelocity; // Aumentamos el ángulo para el siguiente punto
  }

  offset += 0.1; // Hace que la onda fluya en el tiempo como una ola
}
```

- Se mueve la lógica de dibujo a draw() para que la animación sea continua.
- Se usa una variable offset para desplazar la onda horizontalmente con el tiempo, creando el efecto de movimiento de ola.
- Mantiene el cálculo de y con sin(), pero ahora le sumamos offset para el desplazamiento.

![image](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExdzhkMTRhNXc2dmxtYmEzeDNobWQ4eGVpdXR3a2g1djlleWtxcGR4cyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/swoMXBpktD4dHQPJLI/giphy.gif)

[Aplicación](https://editor.p5js.org/Majogc8/sketches/uZMRcNeMr)
