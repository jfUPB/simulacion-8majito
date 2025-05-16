```js
let mover;

function setup() {
    createCanvas(640, 240);
    mover = new Mover();
}

function draw() {
    background(255);
    mover.show();
    mover.update();
    mover.checkEdges();
}

.
.
.


update() {

    // Aquí calculo la aceleración
    .
    .
    .
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
}
.
.
.
```
#### El marco motion 101

El marco Motion 101 se basa en la idea de que el movimiento en simulaciones se maneja con la relación entre posición, velocidad y aceleración. En el código dado, se pueden identificar estos elementos:
- Posición: this.position.add(this.velocity);
- Velocidad: this.velocity.add(this.acceleration);
- Aceleración: Se debe calcular antes de actualizar la velocidad.

Motion 101 establece que:
- La aceleración afecta la velocidad.
- La velocidad afecta la posición.
- Los objetos se mueven con estos principios básicos de movimiento.

#### En la unidad anterior, tú definías la aceleración mediante algún algoritmo. ¿Cuáles eran?
- 1. Aceleración constante Esto se logra al asignar un vector de aceleración fijo.
```js
this.acceleration = createVector(0.01, 0); // Aceleración constante hacia la derecha
```

2. Aceleración hacia el mouse:  se calcula como un vector que apunta hacia la posición del mouse.
```js
let mouse = createVector(mouseX, mouseY);
this.acceleration = p5.Vector.sub(mouse, this.position);
this.acceleration.setMag(0.1); // Controlamos la magnitud
```

3. Aceleración aleatoria: Cada frame se genera una aceleración en una dirección aleatoria.
```js
this.acceleration = p5.Vector.random2D();
this.acceleration.mult(0.5); // Reducimos la magnitud
```

#### ¿Qué tiene que ver esto con las leyes de movimiento de Newton?

La aceleración está directamente relacionada con la Segunda Ley de Newton, que es F= m x a. Esto significa que la aceleración de un objeto depende de la fuerza aplicada sobre él y de su masa. En el codio se presenta asi: 

```js
let force = createVector(0.2, 0); // Fuerza aplicada
this.acceleration = p5.Vector.div(force, this.mass); // a = F / m
```
- En esta unidad, en lugar de definir la aceleración directamente, se calcula a partir de las fuerzas.
