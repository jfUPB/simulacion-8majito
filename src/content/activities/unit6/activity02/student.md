### Flow Fields 

#### 1. Identificación de la estructura del campo en el código

*¿Qué estructura de datos se usa?*
- El campo de flujo se almacena en un array 2D (this.field):

```js
this.field = new Array(this.cols);
for (let i = 0; i < this.cols; i++) {
  this.field[i] = new Array(this.rows);
}
```
- Cada celda de este array almacena un vector (p5.Vector) que representa la dirección del flujo en esa ubicación.

*¿Qué representa cada elemento de esa estructura?*
  - Cada elemento this.field[i][j] es un vector de dirección que indica hacia dónde se moverán los objetos que sigan el flujo en esa celda.
  - Se genera a partir de Perlin noise, que crea transiciones suaves en el campo.
```js
let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
this.field[i][j] = p5.Vector.fromAngle(angle);
```
- El angle se obtiene con noise(xoff, yoff), asegurando que los vectores cambien suavemente en el espacio.

*¿Cómo se calcula inicialmente el vector en cada punto?*
- Se usa Perlin noise para generar valores entre 0 y 1, que luego se mapean a un ángulo entre 0 y TWO_PI (0° a 360°).
- Ese ángulo se convierte en un vector unitario con p5.Vector.fromAngle(angle).

```js
let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
this.field[i][j] = p5.Vector.fromAngle(angle);
```
- noise(xoff, yoff) genera un valor pseudoaleatorio suave.
- map(..., 0, 1, 0, TWO_PI) convierte ese valor en un ángulo.
- p5.Vector.fromAngle(angle) crea un vector en esa dirección.

```js
class FlowField {
  constructor(r) {
    this.resolution = r; // Tamaño de cada celda en la cuadrícula
    this.cols = width / this.resolution; // Número de columnas
    this.rows = height / this.resolution; // Número de filas

    // Crear un array 2D de vectores
    this.field = new Array(this.cols);
    for (let i = 0; i < this.cols; i++) {
      this.field[i] = new Array(this.rows);
    }

    this.init(); // Llamar a la función que inicializa los vectores
  }

  // Inicializar los vectores en la cuadrícula
  init() {
    noiseSeed(random(10000)); // Semilla para Perlin noise
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        // Usar Perlin noise para generar un ángulo entre 0 y 2*PI
        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        // Crear un vector a partir del ángulo
        this.field[i][j] = p5.Vector.fromAngle(angle);
        yoff += 0.1; // Incremento para Perlin noise en Y
      }
      xoff += 0.1; // Incremento para Perlin noise en X
    }
  }

  // Función para obtener el vector en una posición específica
  lookup(position) {
    let column = constrain(floor(position.x / this.resolution), 0, this.cols - 1);
    let row = constrain(floor(position.y / this.resolution), 0, this.rows - 1);
    return this.field[column][row].copy(); // Devuelve una copia del vector en esa celda
  }

  // Dibujar el campo de flujo
  show() {
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let w = width / this.cols;
        let h = height / this.rows;
        let v = this.field[i][j].copy();
        v.setMag(w * 0.5); // Escalar el vector para visualizarlo
        let x = i * w + w / 2;
        let y = j * h + h / 2;
        strokeWeight(1);
        line(x, y, x + v.x, y + v.y); // Dibujar el vector como una línea
      }
    }
  }
}
```
