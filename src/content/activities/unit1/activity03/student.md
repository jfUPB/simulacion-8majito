## Caminata aleatoria
Proceso en el que un "caminante" se mueve paso a paso en direcciones aleatorias dentro de un espacio definido. En cada paso, la dirección se elige al azar, y puede ser hacia arriba, abajo, izquierda o derecha (en el caso más simple).
En programación, una caminata aleatoria se crea usando un bucle donde en cada iteración se calcula la nueva posición basándose en una dirección aleatoria. Esto genera trayectorias que pueden parecer caóticas o dispersas dependiendo de las reglas del movimiento.

### Comprensión de código: 

**Clase Walker:**
Es el objeto principal que representa al "caminante".
Tiene dos propiedades (this.x y this.y) que marcan su posición actual en el lienzo, empezando en el centro.

**Método step():**
Define cómo se mueve el caminante.

Usa un número aleatorio entre 0 y 3 para decidir la dirección:

0: Mueve hacia la derecha (this.x++).

1: Mueve hacia la izquierda (this.x--).

2: Mueve hacia abajo (this.y++).

3: Mueve hacia arriba (this.y--).

**Método show():**
Dibuja un punto en la posición actual del caminante.
Bucle draw():

Se ejecuta continuamente, haciendo que el caminante dé un paso (step()) y se dibuje en su nueva posición (show()).

```js
// Declara una variable para almacenar el objeto "Walker"
let walker;

function setup() {
  // Crea un lienzo de 640x240 píxeles
  createCanvas(640, 240);

  // Inicializa un nuevo caminante (instancia de la clase Walker)
  walker = new Walker();

  // Establece el fondo en color blanco
  background(255);
}

function draw() {
  // Llama al método "step()" para que el caminante dé un paso
  walker.step();

  // Llama al método "show()" para dibujar al caminante en su nueva posición
  walker.show();
}

// Definición de la clase "Walker"
class Walker {
  // Constructor: inicializa la posición del caminante en el centro del lienzo
  constructor() {
    this.x = width / 2; // Coordenada X: mitad del ancho del lienzo
    this.y = height / 2; // Coordenada Y: mitad de la altura del lienzo
  }

  // Método "show()": dibuja al caminante en su posición actual
  show() {
    stroke(0); // Establece el color del punto en negro
    point(this.x, this.y); // Dibuja un punto en la posición actual (x, y)
  }

  // Método "step()": define cómo se mueve el caminante
  step() {
    // Genera un número entero aleatorio entre 0 y 3
    const choice = floor(random(4));

    // Elige una dirección de movimiento basada en el valor de "choice"
    if (choice == 0) {
      this.x++; // Si "choice" es 0, el caminante se mueve hacia la derecha (aumenta X)
    } else if (choice == 1) {
      this.x--; // Si "choice" es 1, el caminante se mueve hacia la izquierda (disminuye X)
    } else if (choice == 2) {
      this.y++; // Si "choice" es 2, el caminante se mueve hacia abajo (aumenta Y)
    } else {
      this.y--; // Si "choice" es 3, el caminante se mueve hacia arriba (disminuye Y)
    }
  }
}
```

### Experimento con el código:

Comprende el código, entiende lo que pasa y cómo se está aplicando el concepto.
Luego harás una modificación al código. Te harás esta pregunta ¿Qué pasaría si?
Trata de lanzar una hipótesis de lo que crees que ocurrirá.
Realiza la modificación.
Vas a documentar

## Describe el experimento que vas a realizar.

#### ¿Qué pregunta quieres responder con este experimento?
#### ¿Qué resultados esperas obtener?
#### ¿Qué resultados obtuviste?
#### ¿Qué aprendiste de este experimento?
