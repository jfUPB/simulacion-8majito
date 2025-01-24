#### En qué consiste el concepto de Lévy flight y en qué caso sería interesante usarlo.

Un Lévy flight es un patrón de movimiento o de salto caracterizado por una secuencia de desplazamientos en los cuales los tamaños de los saltos siguen una distribución Lévy (una distribución de probabilidad con colas pesadas). Esta distribución se diferencia de la distribución normal porque tiene una mayor probabilidad de generar saltos muy largos en comparación con los saltos pequeños. Los saltos cortos son más frecuentes, pero las trayectorias pueden incluir desplazamientos muy grandes, lo que crea un patrón errático y no lineal.

**¿Cuándo es interesante usarlo?**

El Lévy flight es especialmente útil en modelos de búsqueda y exploración donde se quiere simular un comportamiento errático pero eficiente, como:

- En biología, para modelar el movimiento de animales que buscan alimentos, como las abejas o algunos tipos de peces.
- En algoritmos de optimización, como el Lévy flight search.
- En física y estudios de difusión, como el movimiento de partículas en medios heterogéneos.

#### Código de la simulación.

let x, y;

function setup() {
  createCanvas(640, 240);
  x = width / 2;
  y = height / 2;
  background(255);
  noFill();
  stroke(0, 50, 255); // Línea de color
  frameRate(15); // Hacer que la animación sea moderadamente lenta
}

function draw() {
  // Simulación de Lévy flight
  let angle = random(TWO_PI); // Ángulo aleatorio
  let stepSize = pow(random(1), -1 / 1.5) * 30; // Aumentar el tamaño de los pasos
  
  // Calculando el nuevo desplazamiento
  let dx = cos(angle) * stepSize;
  let dy = sin(angle) * stepSize;
  
  // Dibujar línea desde la última posición hasta la nueva posición
  line(x, y, x + dx, y + dy);
  
  // Actualizando las posiciones
  x += dx;
  y += dy;
  
  // Dibujar un corazón en la nueva posición
  drawHeart(x, y);
  
  // Limitar la visualización a un tamaño de canvas
  if (x < 0 || x > width || y < 0 || y > height) {
    background(255); // Si se sale del canvas, reinicia
    x = width / 2;
    y = height / 2;
  }
}

// Función para dibujar un corazón
function drawHeart(x, y) {
  beginShape();
  vertex(x, y);
  bezierVertex(x - 10, y - 20, x - 30, y - 20, x - 30, y);
  bezierVertex(x - 30, y + 30, x, y + 50, x, y + 30);
  bezierVertex(x + 30, y + 50, x + 30, y + 30, x + 30, y);
  bezierVertex(x + 30, y - 20, x + 10, y - 20, x, y);
  endShape(CLOSE);
}


#### Captura de pantalla.

![image](https://github.com/user-attachments/assets/ad79ed65-54c7-48ef-a583-92e4723ec71a)
