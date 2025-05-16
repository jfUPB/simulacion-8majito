#### Experimento 

**Aceleración constante.**
```js
let mover;

function setup() {
  createCanvas(640, 480);
  mover = new Mover();
}

function draw() {
  background(255);
  mover.update();
  mover.checkEdges();
  mover.show();
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2); // Posición inicial en el centro
    this.velocity = createVector(0, 0); // Velocidad inicial en 0
    this.acceleration = createVector(0.1, 0.1); // Aceleración constante en x y y
  }

  update() {
    this.velocity.add(this.acceleration); // Aumenta la velocidad con la aceleración
    this.position.add(this.velocity); // Mueve la posición con la velocidad
  }

  checkEdges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1; // Rebote en los bordes horizontales
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1; // Rebote en los bordes verticales
    }
  }

  show() {
    fill(0);
    noStroke();
    circle(this.position.x, this.position.y, 20); // Dibuja la bola
  }
}
```

La aceleración constante hace que la velocidad se incremente progresivamente, lo que provoca un movimiento cada vez más rápido. Al chocar con los bordes, la dirección de la velocidad se invierte.

**Aceleración aleatoria.**
```js
let mover;

function setup() {
  createCanvas(640, 480);
  mover = new Mover();
}

function draw() {
  background(255);
  mover.update();
  mover.checkEdges();
  mover.show();
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2); // Comienza en el centro
    this.velocity = createVector(0, 0); // Velocidad inicial en 0
  }

  update() {
    let acceleration = createVector(random(-1, 1), random(-1, 1)); // Aceleración con cambios más bruscos
    this.velocity.add(acceleration); // La velocidad cambia con cada frame
    this.velocity.limit(8); // Limitamos la velocidad máxima
    this.position.add(this.velocity); // Se mueve según la velocidad
  }

  checkEdges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1; // Rebote en los bordes
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }

  show() {
    fill(0);
    noStroke();
    circle(this.position.x, this.position.y, 20);
  }
}
```
La aceleración aleatoria cambia en cada frame, lo que hace que la dirección del movimiento sea impredecible. Esto imita el comportamiento de partículas en un fluido o incluso el movimiento de algunas bacterias.
- Cada frame, la aceleración cambia aleatoriamente en un pequeño rango.
- La velocidad se ajusta con la aceleración, causando que la bola se mueva de forma errática.
- El objeto rebota en los bordes, invirtiendo su dirección.
**Aceleración hacia el mouse.**
```js
let mover;

function setup() {
  createCanvas(640, 480);
  mover = new Mover();
}

function draw() {
  background(255);
  mover.update();
  mover.checkEdges();
  mover.show();
}

class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2); // Comienza en el centro
    this.velocity = createVector(0, 0); // Velocidad inicial en 0
  }

  update() {
    let mouse = createVector(mouseX, mouseY); // Posición del mouse
    let acceleration = p5.Vector.sub(mouse, this.position); // Calcula el vector hacia el mouse
    acceleration.setMag(0.1); // Define la magnitud de la aceleración

    this.velocity.add(acceleration); // Agrega aceleración a la velocidad
    this.velocity.limit(5); // Limita la velocidad máxima
    this.position.add(this.velocity); // Mueve la posición
  }

  checkEdges() {
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1; // Rebote en los bordes
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }

  show() {
    fill(0);
    noStroke();
    circle(this.position.x, this.position.y, 20);
  }
}
```
- Se calcula un vector desde la bola hasta el mouse con p5.Vector.sub(mouse, this.position).
- La aceleración se establece con setMag(0.1), lo que significa que siempre será una fuerza constante, pero en la dirección del mouse.
- La velocidad aumenta en la dirección del mouse y se limita a 5 para evitar que sea demasiado rápida.
- Si la bola choca con los bordes, se rebota.
  
#### Explicación frase

***The goal for programming motion is to come up with an algorithm for calculating acceleration and then let the trickle-down effect work its magic.
La frase dice que el objetivo al programar el movimiento es encontrar un algoritmo para calcular la aceleración, y luego dejar que el "trickle-down effect" haga su magia.***

¿Qué significa esto en nuestros experimentos?
- Primero definimos cómo cambia la aceleración (constante, aleatoria o dirigida al mouse).
- Luego, esa aceleración afecta la velocidad (hace que aumente o cambie).
- Finalmente, la velocidad modifica la posición del objeto.

Este es el "trickle-down effect" → La aceleración genera cambios en la velocidad y estos afectan el movimiento.

Los experimentos lo demostraron:
- Con aceleración constante → La bola se iba moviendo cada vez más rápido.
- Con aceleración aleatoria → La bola cambiaba de dirección de manera errática.

La clave en la animación de movimiento no es solo la velocidad, sino cómo se calcula y aplica la aceleración. 
