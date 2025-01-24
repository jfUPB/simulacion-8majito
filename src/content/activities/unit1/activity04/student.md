#### Explicación de la diferencia entre distribuciones uniformes y no uniformes:

**Distribución uniforme:** Todos los números dentro de un rango tienen la misma probabilidad de ser seleccionados. En el código del ejemplo, random(100) genera números entre 0 y 100 con igual probabilidad, lo que distribuye los puntos de manera uniforme a lo largo de la línea horizontal.

**Distribución no uniforme (gaussiana):** Los números se distribuyen alrededor de un valor central (media) con una mayor probabilidad cerca de la media y menos probabilidad en los extremos. En el ejemplo, randomGaussian(50) genera números con una distribución alrededor del 50, lo que concentra más puntos cerca de ese valor, creando un agrupamiento central.

#### Codigo caminata con distribución no uniforme:
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
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const stepX = randomGaussian(1, 1); // Favorece movimientos a la derecha (media 1)
    const stepY = random(-1, 1); // Distribución uniforme para movimientos verticales

    this.x += stepX;
    this.y += stepY;

    // Asegurar que se mantenga dentro de los límites
    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }
}

![image](https://github.com/user-attachments/assets/23686567-95ba-482b-b863-11cf6aada059)
