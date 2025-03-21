**1. Clase DoublePendulum**
- Esta clase representa el doble péndulo y contiene toda la lógica para su simulación, dibujo y manejo de interacción.

**2. Metodo Update**
- cCalcula cómo se mueven los ángulos del doble péndulo usando fórmulas físicas basadas en las ecuaciones del movimiento.
- Si no sé esta arrastrando las masas (!this.dragging1 && !this.dragging2), se aplican las leyes de la dinámica al sistema.

**3. Metodo Show**
- dibuja las cuerdas, las masas y la traza que deja el movimiento del segundo péndulo.

**4.Interacciones**
- drag: Actualiza los ángulos si estás arrastrando una masa.
- clicked(mx, my): Detecta si hiciste clic cerca de una masa y activa el arrastre.
- stopDragging: Finaliza el arrastre cuando se suelta el mouse.

```js
class DoublePendulum {
 constructor(x, y, r1, r2) {
    this.pivot = createVector(x, y); // Punto de pivote (la parte fija donde cuelga el péndulo).
    this.r1 = r1; // Longitud de la primera cuerda.
    this.r2 = r2; // Longitud de la segunda cuerda.
    this.angle1 = PI / 4; // Ángulo inicial del primer péndulo.
    this.angle2 = PI / 2; // Ángulo inicial del segundo péndulo.
    this.angleVelocity1 = 0.0; // Velocidad angular del primer péndulo.
    this.angleVelocity2 = 0.0; // Velocidad angular del segundo péndulo.
    this.angleAcceleration1 = 0.0; // Aceleración angular del primer péndulo.
    this.angleAcceleration2 = 0.0; // Aceleración angular del segundo péndulo.
    this.m1 = 10; // Masa del primer péndulo.
    this.m2 = 10; // Masa del segundo péndulo.
    this.g = 1; // Gravedad (puedes ajustarla para hacerlo más rápido o lento).
    this.dragging1 = false; // Indica si el primer péndulo está siendo arrastrado.
    this.dragging2 = false; // Indica si el segundo péndulo está siendo arrastrado.
    this.prevX2 = null; // Última posición en X del segundo péndulo (para la traza).
    this.prevY2 = null; // Última posición en Y del segundo péndulo (para la traza).
    this.trailColor = color(0, 255, 0, 150); // Color de la traza (verde semitransparente).
}

  update() {
    if (!this.dragging1 && !this.dragging2) {
      let num1 = -this.g * (2 * this.m1 + this.m2) * sin(this.angle1);
      let num2 = -this.m2 * this.g * sin(this.angle1 - 2 * this.angle2);
      let num3 = -2 * sin(this.angle1 - this.angle2) * this.m2;
      let num4 = this.angleVelocity2 ** 2 * this.r2 + this.angleVelocity1 ** 2 * this.r1 * cos(this.angle1 - this.angle2);
      let den = this.r1 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.angle1 - 2 * this.angle2));
      this.angleAcceleration1 = (num1 + num2 + num3 * num4) / den;

      let num5 = 2 * sin(this.angle1 - this.angle2);
      let num6 = this.angleVelocity1 ** 2 * this.r1 * (this.m1 + this.m2) + this.g * (this.m1 + this.m2) * cos(this.angle1);
      let num7 = this.angleVelocity2 ** 2 * this.r2 * this.m2 * cos(this.angle1 - this.angle2);
      let den2 = this.r2 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.angle1 - 2 * this.angle2));
      this.angleAcceleration2 = (num5 * (num6 + num7)) / den2;

      this.angleVelocity1 += this.angleAcceleration1;
      this.angleVelocity2 += this.angleAcceleration2;
      this.angle1 += this.angleVelocity1;
      this.angle2 += this.angleVelocity2;
    }
  }

  show() {
    let x1 = this.pivot.x + this.r1 * sin(this.angle1);
    let y1 = this.pivot.y + this.r1 * cos(this.angle1);
    let x2 = x1 + this.r2 * sin(this.angle2);
    let y2 = y1 + this.r2 * cos(this.angle2);

    stroke(255);
    strokeWeight(2);
    fill(255);
    line(this.pivot.x, this.pivot.y, x1, y1);
    line(x1, y1, x2, y2);
    ellipse(x1, y1, this.m1 * 2);
    ellipse(x2, y2, this.m2 * 2);

    if (this.prevX2 !== null && this.prevY2 !== null) {
      trailGraphics.stroke(this.trailColor);
      trailGraphics.strokeWeight(2);
      trailGraphics.line(this.prevX2, this.prevY2, x2, y2);
    }

    this.prevX2 = x2;
    this.prevY2 = y2;
  }

  drag() {
    let x1 = this.pivot.x + this.r1 * sin(this.angle1);
    let y1 = this.pivot.y + this.r1 * cos(this.angle1);
    let x2 = x1 + this.r2 * sin(this.angle2);
    let y2 = y1 + this.r2 * cos(this.angle2);

    if (this.dragging1) {
      this.angle1 = atan2(mouseY - this.pivot.y, mouseX - this.pivot.x) - PI / 2;
      this.angleVelocity1 = 0;
    } else if (this.dragging2) {
      this.angle2 = atan2(mouseY - y1, mouseX - x1) - PI / 2;
      this.angleVelocity2 = 0;
    }
  }

  clicked(mx, my) {
    let x1 = this.pivot.x + this.r1 * sin(this.angle1);
    let y1 = this.pivot.y + this.r1 * cos(this.angle1);
    let x2 = x1 + this.r2 * sin(this.angle2);
    let y2 = y1 + this.r2 * cos(this.angle2);

    // Hacer que las áreas de detección sean más grandes
    if (dist(mx, my, x1, y1) < this.m1 * 2) {
      this.dragging1 = true;
    } else if (dist(mx, my, x2, y2) < this.m2 * 2) {
      this.dragging2 = true;
    }
  }

  stopDragging() {
    this.dragging1 = false;
    this.dragging2 = false;
  }
}


let doublePendulum;
let trailGraphics;

function setup() {
  createCanvas(640, 480);
  background(0); // Fondo negro
  trailGraphics = createGraphics(width, height);
  trailGraphics.background(0);
  trailGraphics.stroke(0, 255, 0, 150); // Color del trazo en verde
  trailGraphics.strokeWeight(2);

  // Crear un péndulo doble
  doublePendulum = new DoublePendulum(width / 2, 200, 125, 125);
}

function draw() {
  background(0); // Fondo negro
  image(trailGraphics, 0, 0); // Dibujar el rastro
  doublePendulum.update();
  doublePendulum.show();
}
```
[App](https://editor.p5js.org/Majogc8/sketches/Da0_P2oEBm)

![Image](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExdTFzNTgycnIyZzZyMzNvM3N2dGtvYmJmcXMzbzBiM3YwejZoMWJrbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/WZDPTiptJZCOv9YquU/giphy.gif)
