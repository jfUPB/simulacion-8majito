#### ¿Qué relación tienen las leyes de Newton con el arte generativo?

El arte generativo se basa en reglas y sistemas para crear imágenes, animaciones o estructuras visuales. Las leyes de Newton nos permiten diseñar sistemas donde los elementos gráficos reaccionan de forma natural a fuerzas simuladas, como la gravedad, el viento o la atracción entre partículas.

En la unidad anterior, definíamos la aceleración directamente en cada frame. Ahora, en lugar de establecerla manualmente, la calculamos como la sumatoria de todas las fuerzas que afectan a un objeto, siguiendo la Segunda Ley de Newton:
𝐹=𝑚⋅𝑎
Si en nuestro mundo de arte generativo decidimos que la masa es 1, la ecuación se simplifica a:
𝑎=𝐹
- Esto significa que la aceleración será exactamente igual a la fuerza aplicada, lo que hace más sencillo calcular el movimiento en nuestras simulaciones.

 #### ¿Qué problema le ves a este planteamiento? 
 El problema con eL código es que la función applyForce(force) está sobrescribiendo el valor de this.acceleration en lugar de sumarlo. Esto significa que si aplicamos primero mover.applyForce(wind); y luego mover.applyForce(gravity);, la aceleración solo tomará el valor de gravity, perdiendo completamente el efecto del wind.
 
 ```js
 applyForce(force) {
  this.acceleration = force; // Esto borra cualquier otra fuerza aplicada antes
}
 ```
 
 #### ¿Qué solución propones?
 
 En lugar de sobrescribir la aceleración, debemos sumarle todas las fuerzas aplicadas en cada frame. Esto se logra acumulando las fuerzas en el vector de aceleración:

```js
 applyForce(force) {
  this.acceleration.add(force); // Sumar fuerzas en lugar de reemplazar
}
```
- Entonces la aceleración será la suma de todas las fuerzas que actúan sobre el objeto en ese frame.
  
 #### ¿Cómo lo implementarías en p5.js?

 ```js
 class Mover {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    this.acceleration.add(force); // Se suman todas las fuerzas aplicadas
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Se reinicia la aceleración en cada frame
  }

  show() {
    fill(0);
    ellipse(this.position.x, this.position.y, 20, 20);
  }
}

let mover;

function setup() {
  createCanvas(640, 360);
  mover = new Mover();
}

function draw() {
  background(255);

  let gravity = createVector(0, 0.1);
  let wind = createVector(0.05, 0);

  mover.applyForce(gravity);
  mover.applyForce(wind);

  mover.update();
  mover.show();
}
 ```
- applyForce(force) → Suma la fuerza a la aceleración en lugar de sobrescribirla.
- update()
  - La aceleración se suma a la velocidad.
  - La velocidad se suma a la posición.
  - Se reinicia la aceleración (acceleration.mult(0);) para el siguiente frame.
- Se aplican múltiples fuerzas en cada frame (gravity y wind).
 
