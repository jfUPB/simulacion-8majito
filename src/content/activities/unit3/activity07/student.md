#### ¿Qué problema tiene este planteamiento?
 - *force.div(10)* modifica directamente el vector de fuerza original porque los objetos en JavaScript, como los de la clase p5.Vector, se pasan por referencia y no por valor
 - Esto significa que cuando se divide force por la masa dentro de applyForce(), estamos modificando la fuerza original fuera de la función, lo que puede causar efectos no deseados en otras partes del código.
 - Si otro objeto intenta usar la misma fuerza (por ejemplo, si tenemos múltiples objetos en la escena con la misma fuerza de gravedad o viento), estos se verán afectados de manera incorrecta porque la fuerza ya fue alterada en el primer objeto que la recibió.

#### Solución 
- Crear una copia del vector antes de dividir por la masa, para no modificar la original
- Escalar la fuerza por la masa → En lugar de dividir dentro de applyForce(), primero multiplicamos la fuerza original por la masa de cada objeto y luego la aplicamos.
```js
applyForce(force) {
    let f = force.copy();  // 🔹 Creamos una copia de la fuerza para no modificar la original
    f.div(this.mass);      // 🔹 Escalamos la fuerza por la masa del objeto
    this.acceleration.add(f);
}
```

#### Ejemplo en p5.js
en codigo de mover js
```js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = force.copy(); // Se copia la fuerza para evitar modificar la original
    f.div(this.mass); // Se divide la fuerza por la masa
    this.acceleration.add(f); // Se acumula la fuerza en la aceleración
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Se resetea la aceleración cada frame
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= -1;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= -1;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= -1;
    }
  }
}
```
https://editor.p5js.org/Majogc8/sketches/Rld4lWKDK
