### Ejemplo 0.1: Paseo aleatorio tradicional 

#### ¿Que tuviste que hacer para hacer la conversión propuesta?
- En lugar de tener las variables *this.x y this.y* como números independientes, use un p5.Vector que almacene ambas coordenadas.
- **Modificación de step():**
Cuando el caminante elige una dirección, vamos a sumarle un vector de desplazamiento. Por ejemplo, si el caminante se mueve a la derecha, agregamos un vector (1, 0).
- **Actualización de la posición:**
En lugar de modificar this.x y this.y por separado, actualicé el vector de posición sumando un nuevo vector de acuerdo a la dirección elegida.


let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    // Inicializamos el vector de posición en el centro del lienzo
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y); // Dibujamos el punto usando el vector
  }

  step() {
    // Elegimos una dirección aleatoria
    const choice = floor(random(4));

    let step = createVector(0, 0); // Creamos un vector de paso vacío

    if (choice == 0) {
      step.x = 1; // Movimiento a la derecha
    } else if (choice == 1) {
      step.x = -1; // Movimiento a la izquierda
    } else if (choice == 2) {
      step.y = 1; // Movimiento hacia abajo
    } else {
      step.y = -1; // Movimiento hacia arriba
    }

    // Sumamos el vector de paso a la posición actual
    this.position.add(step);
  }
}
