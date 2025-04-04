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
#### Analiza el comportamiento del agente:

*¿Cómo determina el agente qué vector del campo de flujo debe seguir?

-  En el campo de flujo como una cuadrícula gigante llena de flechitas que indican hacia dónde moverse. El agente está en algún punto de la pantalla y necesita saber qué flechita seguir.  
 - Divide por el tamaño de cada cuadrito de la cuadrícula y así sabe en qué celda está. Básicamente, convierte su posición en coordenadas de la cuadrícula y busca la flechita que le toca.

*¿Cómo usa esa flechita para moverse?*

Ya que tiene la flechita (vector), la usa para calcular hacia dónde debe girar y cuánto debe acelerar. Una vez que tiene esa dirección, la ajusta a su velocidad máxima y la compara con su velocidad actual. Si ya va en esa dirección, casi no cambia, pero si va en otra, calcula cuánto necesita girar y ajusta su rumbo poco a poco para no hacer movimientos bruscos. Luego aplica esa fuerza a su movimiento y sigue fluyendo de manera suave.
Ya que tiene la flechita (vector), la usa para calcular hacia dónde debe girar y cuánto debe acelerar.

#### Identifica parámetros clave

*La resolución del campo de flujo:* Está determinada por this.resolution en la clase FlowField. Se define en flowfield = new FlowField(20);, lo que significa que cada celda de la cuadrícula tiene un tamaño de 20 píxeles.

```js 
class FlowField {
  constructor(r) {
    this.resolution = r;
    //{!2} Determine the number of columns and rows.
    this.cols = width / this.resolution;
    this.rows = height / this.resolution;
```

Dentro de la clase Vehicle puedes ver los parámetros clave que controlan la velocidad y la fuerza de los agentes:

- *Velocidad máxima (maxspeed):* Está en el constructor de la clase y se pasa como parámetro al crear un vehículo.

```js
this.maxspeed = ms;
```
Esto significa que cada vehículo puede tener una velocidad máxima diferente, dependiendo del valor que se le pase en new Vehicle(...).

- *Fuerza máxima (maxforce):* También está en el constructor y define cuánto puede cambiar su dirección el vehículo en un instante.

```
this.maxforce = mf;
```

Estos valores se usan en la función follow(flow), donde:

- desired.mult(this.maxspeed); → Asegura que el vector de dirección tenga la magnitud de la velocidad máxima.

- steer.limit(this.maxforce); → Restringe la fuerza de dirección para evitar cambios bruscos.

- Básicamente, estos dos parámetros controlan qué tan rápido se mueven los vehículos y qué tanto pueden girar en cada instante.

#### Experimentación con modificaciones

Cambie la forma en que se generan los vectores del campo de flujo para que no sean estáticos, use ondas sinusoidales y cosenoidales, lo que generará un patrón más estructurado y repetitivo en el campo de flujo.

*¿Qué hacía el código original?*

En el código original, se usaba Perlin Noise para generar los ángulos de los vectores:

```
let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
```

*¿Qué cambiamos?*

Ahora usamos sin() y cos() en función de la posición (i, j) para generar el ángulo:
```js
let angle = sin(i * frequency + timeOffset) * cos(j * frequency + timeOffset) * TWO_PI;
```
- Ya no usamos Perlin Noise → Ahora los vectores siguen un patrón basado en funciones trigonométricas.
- Agregamos timeOffset = random(1000)
- Antes, los ángulos siempre dependían de (i, j), ahora también dependen de este número aleatorio.
- Los patrones cambian más bruscamente al hacer clic, porque timeOffset modifica la forma en que se generan los ángulos.

[Modificación](https://editor.p5js.org/Majogc8/sketches/befFBzpqB)

![Image](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExMG5idmJoNTJ3YTg0emxrc2J4NGcyc2J2eTR4MmtkZjJzeTZxb3ZydCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Bs9e0jCB4llxQlFvLA/giphy.gif)







