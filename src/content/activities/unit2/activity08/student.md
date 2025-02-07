#### Entendimiento del ejemplo.

- Crea un objeto Mover que se mueve solo.
- Cuando toca los bordes, aparece en el lado opuesto (como un juego de Pac-Man).
- Se actualiza y se redibuja en cada frame.
```js
  // Declaramos la variable global para el objeto Mover.
let mover;

function setup() {
  createCanvas(640, 240); // Creamos un lienzo de 640x240 píxeles.
  mover = new Mover(); // Inicializamos el objeto Mover.
}

function draw() {
  background(255); // Limpiamos la pantalla con un fondo blanco en cada frame.
  
  mover.update(); // Actualizamos la posición del objeto.
  mover.checkEdges(); // Verificamos si el objeto toca los bordes.
  mover.show(); // Dibujamos el objeto en su nueva posición.
}

class Mover {
  constructor() {
    // La posición inicial será un punto aleatorio dentro del canvas.
    this.position = createVector(random(width), random(height));

    // La velocidad inicial será un vector con valores aleatorios entre -2 y 2 en x e y.
    this.velocity = createVector(random(-2, 2), random(-2, 2));
  }

  update() {
    // La posición cambia sumando la velocidad en cada frame.
    this.position.add(this.velocity);
  }

  show() {
    stroke(0); // Borde negro.
    strokeWeight(2); // Grosor del borde.
    fill(127); // Relleno gris.
    circle(this.position.x, this.position.y, 48); // Dibujamos el círculo en su posición actual.
  }

  checkEdges() {
    // Si la posición X supera el ancho del lienzo, reaparece en X = 0.
    if (this.position.x > width) {
      this.position.x = 0;
    } 
    // Si la posición X es menor a 0, reaparece en X = ancho del lienzo.
    else if (this.position.x < 0) {
      this.position.x = width;
    }

    // Si la posición Y supera la altura del lienzo, reaparece en Y = 0.
    if (this.position.y > height) {
      this.position.y = 0;
    } 
    // Si la posición Y es menor a 0, reaparece en Y = altura del lienzo.
    else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```
#### Vas a proponer una modificación mediante esta pregunta que te harás: ¿Qué pasa si…?

¿Qué pasa si las bolas cambian de color al cruzar los bordes y dejan un rastro de su movimiento?

#### ¿Qué te imaginas que pasará?

 Espero que las bolas se muevan aleatoriamente y, cada vez que salgan del canvas, reaparezcan del otro lado con un color diferente. Como el fondo no se borra, deberían dejar un rastro de colores en la pantalla, y va a ser una combinación de muchos colores

#### ¿Qué pasó?

Las bolas efectivamente cambian de color cada vez que cruzan los bordes. También se va formando un patrón visual de líneas de colores debido a que no se borra el fondo en draw()

#### ¿Por qué?

Cuando una bola llega al borde del canvas, se reposiciona del otro lado, y en ese momento cambia su color. Como el fondo no se reinicia, su trayectoria se va dibujando en la pantalla.

#### Conclusión

Este experimento muestra cómo se puede generar un efecto visual interesante simplemente modificando la forma en que se actualiza la posición y el color de los objetos. Además, demuestra cómo la persistencia del rastro crea un efecto artístico llamativo.

#### Codigo 

```js
let movers = [];
let numMovers = 5; // Cantidad inicial de bolas

function setup() {
  createCanvas(640, 480);
  for (let i = 0; i < numMovers; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  // No usamos background para dejar el rastro de movimiento.
  for (let mover of movers) {
    mover.update();
    mover.checkEdges();
    mover.show();
  }
}

class Mover {
  constructor() {
    // Posición y velocidad aleatoria.
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(random(-7, 5), random(-7, 5));
    this.color = color(random(255), random(255), random(255)); // Color inicial aleatorio.
  }

  update() {
    this.position.add(this.velocity);
  }

  checkEdges() {
    let rebote = false;

    if (this.position.x > width) {
      this.position.x = 0;
      rebote = true;
    } else if (this.position.x < 0) {
      this.position.x = width;
      rebote = true;
    }

    if (this.position.y > height) {
      this.position.y = 0;
      rebote = true;
    } else if (this.position.y < 0) {
      this.position.y = height;
      rebote = true;
    }

    // Si la bola sale del canvas, cambia de color.
    if (rebote) {
      this.color = color(random(255), random(255), random(255));
    }
  }

  show() {
    fill(this.color);
    noStroke();
    circle(this.position.x, this.position.y, 20);
  }
}
```
![image](https://github.com/user-attachments/assets/07b46b71-217a-4044-9115-d796daa30542)


![image](../../../../)
