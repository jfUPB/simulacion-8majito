#### Explicación
**A graph of Perlin noise values over time (left) and of random noise values over time (right)”**

Figura 0.4:

**Gráfico izquierdo (ruido de Perlin):**
Este gráfico muestra cómo los números cambian de manera suave y continua. Los valores no son totalmente aleatorios, sino que están relacionados entre sí. Es como si los números "trabajaran en equipo" para formar una línea fluida, como las olas del mar o el relieve de una montaña.Este comportamiento es ideal para simular fenómenos naturales.

**Gráfico derecho (aleatoriedad uniforme):**
Representa una secuencia de valores puramente aleatorios, donde cada número no tiene relación con los demás. Esto muestra un gráfico irregular, con saltos bruscos entre los puntos. Aquí los números cambian de forma caótica y desordenada.

En resumen, el gráfico de la izquierda muestra variaciones más naturales y orgánicas, mientras que el de la derecha es más caótico.

#### Explica cómo usaste el ruido Perlin para generar las variaciones.

En el código, use dos tipos de ruido para generar variaciones en la altura de los puntos en las dos gráficas que se dibujan en el lienzo: ruido Perlin (suave) y ruido aleatorio (brusco).

**1. Ruido Perlin:**

En la función dibujarRuidoPerlin(), utilice el ruido Perlin para generar una línea suave y fluida. El ruido Perlin se utiliza para determinar las variaciones en el eje Y de cada punto, generando un cambio gradual entre los puntos.
```js
let y = noise(i * scale) * 150 + yOffset;
```
- *noise(i * scale):* Aquí utilice la función noise() de p5.js, que genera valores continuos (sin saltos abruptos). Al multiplicar i (el índice del punto) por scale (0.01), controlamos la "resolución" de la variación del ruido. Un valor pequeño de scale como este hace que las variaciones sean suaves y amplias.
- **150:* Escala el valor generado por noise() para aumentar la amplitud de las variaciones en el eje Y. El rango de variación se extiende entre 0 y 150 píxeles verticalmente.
- *+ yOffset:* Desplaza la línea en el eje Y, asegurando que la línea no quede demasiado cerca de los bordes del canvas.
Esta variación suave se genera porque el ruido Perlin produce cambios continuos y graduales, lo que da lugar a una línea que parece más natural y orgánica.

**2. Ruido Aleatorio:**

En la función dibujarRuidoAleatorio(), se genera una variación más brusca usando valores aleatorios. Aquí, los puntos en la línea se colocan en posiciones Y aleatorias dentro de un rango determinado.
```js
let y = random(height / 4, (3 * height) / 4);
```
- *random(height / 4, (3 * height) / 4):* En lugar de usar noise(), aquí se usa la *función random()* para generar valores completamente aleatorios. Estos valores se ubican entre height / 4 y (3 * height) / 4, lo que significa que los puntos se distribuyen de manera aleatoria dentro de la mitad del canvas verticalmente.
- Como el ruido aleatorio no tiene en cuenta las posiciones anteriores, genera un comportamiento más "caótico" y con saltos bruscos entre los valores de y.

#### Código
```js
let numPoints = 500; // Número de puntos en la línea
let scale = 0.01; // Escala del ruido Perlin
let yOffset = 150; // Posición base en Y

function setup() {
  createCanvas(600, 400); // Tamaño del canvas
  background(255); // Fondo blanco
  strokeWeight(2); // Grosor de las líneas
  noLoop(); // Solo se ejecuta una vez
}

function draw() {
  background(255); // Limpiar la pantalla

  // Dibujar las líneas de ruido en dos secciones
  dibujarRuidoPerlin(0, 0); // Dibuja la gráfica de ruido Perlin en la mitad izquierda
  dibujarRuidoAleatorio(width / 2, 0); // Dibuja la gráfica de ruido aleatorio en la mitad derecha

  // Etiquetas
  dibujarEtiquetas();
}

function dibujarRuidoPerlin(xOffset, yOffset) {
  stroke(0, 0, 255); // Azul
  noFill();
  beginShape();
  for (let i = 0; i < numPoints; i++) {
    let x = map(i, 0, numPoints, 0, width / 2); // Usamos solo la mitad del ancho
    let y = noise(i * scale) * 150 + yOffset;
    vertex(x + xOffset, y);
  }
  endShape();
}

function dibujarRuidoAleatorio(xOffset, yOffset) {
  stroke(255, 0, 0); // Rojo
  beginShape();
  for (let i = 0; i < numPoints; i++) {
    let x = map(i, 0, numPoints, 0, width / 2); // Usamos solo la mitad del ancho
    let y = random(height / 4, (3 * height) / 4); // Valores aleatorios
    vertex(x + xOffset, y);
  }
  endShape();
}

function dibujarEtiquetas() {
  fill(0);
  noStroke();
  textSize(16);
  text("Ruido de Perlin (suave)", 20, yOffset - 10);
  text("Ruido aleatorio (brusco)", width / 2 + 20, yOffset - 10);
}
```
#### Captura

![image](https://github.com/user-attachments/assets/1b1c64ce-cfee-4c04-ad65-f51f0367dca3)
