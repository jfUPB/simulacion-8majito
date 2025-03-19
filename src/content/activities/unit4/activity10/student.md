#### Péndulo Doble 🐧

**Clase DoblePendulum**

El constructor inicializa todas las propiedades del doble péndulo, incluyendo la geometría (longitudes y ángulos), las propiedades físicas (masas y gravedad) y las variables para la interacción
```js
class DoublePendulum {
  constructor(x, y, r1, r2) {
    // Propiedades iniciales
    this.pivot = createVector(x, y);  // Punto de pivote fijo
    this.r1 = r1;                     // Longitud del primer péndulo
    this.r2 = r2;                     // Longitud del segundo péndulo
    this.angle1 = PI / 4;             // Ángulo inicial del primer péndulo (45 grados)
    this.angle2 = PI / 2;             // Ángulo inicial del segundo péndulo (90 grados)
    this.angleVelocity1 = 0.0;        // Velocidad angular inicial del primer péndulo
    this.angleVelocity2 = 0.0;        // Velocidad angular inicial del segundo péndulo
    this.angleAcceleration1 = 0.0;    // Aceleración angular inicial del primer péndulo
    this.angleAcceleration2 = 0.0;    // Aceleración angular inicial del segundo péndulo
    this.m1 = 10;                     // Masa del primer péndulo
    this.m2 = 10;                     // Masa del segundo péndulo
    this.g = 1;                       // Constante gravitacional
    
    // Variables para la interacción del mouse
    this.dragging1 = false;           // Si estamos arrastrando el primer péndulo
    this.dragging2 = false;           // Si estamos arrastrando el segundo péndulo
  }

 update() {
    // Si ninguno de los péndulos está siendo arrastrado, actualiza según la física
    if (!this.dragging1 && !this.dragging2) {
      // Ecuaciones diferenciales para el movimiento del doble péndulo
      let num1 = -this.g * (2 * this.m1 + this.m2) * sin(this.angle1);
      let num2 = -this.m2 * this.g * sin(this.angle1 - 2 * this.angle2);
      let num3 = -2 * sin(this.angle1 - this.angle2) * this.m2;
      let num4 =
        this.angleVelocity2 * this.angleVelocity2 * this.r2 +
        this.angleVelocity1 * this.angleVelocity1 * this.r1 * cos(this.angle1 - this.angle2);
      let den =
        this.r1 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.angle1 - 2 * this.angle2));
      this.angleAcceleration1 = (num1 + num2 + num3 * num4) / den;
      
      let num5 = 2 * sin(this.angle1 - this.angle2);
      let num6 =
        this.angleVelocity1 * this.angleVelocity1 * this.r1 * (this.m1 + this.m2) +
        this.g * (this.m1 + this.m2) * cos(this.angle1) +
        this.angleVelocity2 * this.angleVelocity2 * this.r2 * this.m2 * cos(this.angle1 - this.angle2);
      let den2 =
        this.r2 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.angle1 - 2 * this.angle2));
      this.angleAcceleration2 = (num5 * num6) / den2;
      
      // Actualizar velocidades con las aceleraciones calculadas
      this.angleVelocity1 += this.angleAcceleration1;
      this.angleVelocity2 += this.angleAcceleration2;
      
      // Aplicar un poco de amortiguamiento (reduce la energía lentamente)
      this.angleVelocity1 *= 0.999;
      this.angleVelocity2 *= 0.999;
      
      // Actualizar ángulos con las velocidades calculadas
      this.angle1 += this.angleVelocity1;
      this.angle2 += this.angleVelocity2;
    }
  }

calculatePositions() { //Convierte los ángulos en coordenadas cartesianas
    // Posición de la primera masa
    this.x1 = this.r1 * sin(this.angle1) + this.pivot.x;
    this.y1 = this.r1 * cos(this.angle1) + this.pivot.y;
    
    // Posición de la segunda masa
    this.x2 = this.x1 + this.r2 * sin(this.angle2);
    this.y2 = this.y1 + this.r2 * cos(this.angle2);
  }

show() {
    this.calculatePositions();
    
    stroke(0);
    strokeWeight(2);
    fill(127);
    
    // Dibujar el primer péndulo
    line(this.pivot.x, this.pivot.y, this.x1, this.y1);
    circle(this.x1, this.y1, this.m1 * 2);
    
    // Dibujar el segundo péndulo
    line(this.x1, this.y1, this.x2, this.y2);
    circle(this.x2, this.y2, this.m2 * 2);
  }

  clicked(mx, my) { // detecta si el usuario ha hecho clic en alguna de las masas
    // Verificar primero la segunda masa (la que está encima visualmente)
    let d2 = dist(mx, my, this.x2, this.y2);
    if (d2 < this.m2) {
      this.dragging2 = true;
      // Reiniciar velocidades cuando se empieza a arrastrar
      this.angleVelocity1 = 0;
      this.angleVelocity2 = 0;
      return true;
    }
    
    // Luego verificar la primera masa
    let d1 = dist(mx, my, this.x1, this.y1);
    if (d1 < this.m1) {
      this.dragging1 = true;
      // Reiniciar velocidades cuando se empieza a arrastrar
      this.angleVelocity1 = 0;
      this.angleVelocity2 = 0;
      return true;
    }
    
    return false;
  }

  stopDragging() { //desactiva ambas variables de arrastre cuando el usuario suelta el mouse.
    this.dragging1 = false;
    this.dragging2 = false;
  }

  drag() { // actualiza los ángulos según la posición del mouse cuando se arrastra una masa
    if (this.dragging1) {
      // Calcular el nuevo ángulo basado en la posición del mouse
      let dx = mouseX - this.pivot.x;
      let dy = mouseY - this.pivot.y;
      this.angle1 = atan2(dx, dy);
      
      // Actualizar la posición basada en el nuevo ángulo
      this.calculatePositions();
    } 
    else if (this.dragging2) {
      // Calcular el nuevo ángulo basado en la posición del mouse relativa a la primera masa
      let dx = mouseX - this.x1;
      let dy = mouseY - this.y1;
      this.angle2 = atan2(dx, dy);
      
      // Actualizar la posición basada en el nuevo ángulo
      this.calculatePositions();
    }
  }
}



let doublePendulum;

function setup() {
  createCanvas(640, 480);  // Crear un lienzo de 640x480 píxeles
  doublePendulum = new DoublePendulum(width / 2, 100, 125, 125);  // Crear el doble péndulo
}

function draw() {
  background(255);  // Establecer fondo blanco
  
  // Actualizar la física
  doublePendulum.update();
  
  // Actualizar si se está arrastrando
  if (mouseIsPressed) {
    doublePendulum.drag();
  }
  
  // Mostrar el péndulo
  doublePendulum.show();
}

function mousePressed() {
  doublePendulum.clicked(mouseX, mouseY);  // Comprobar si se ha hecho clic en alguna masa
}

function mouseReleased() {
  doublePendulum.stopDragging();  // Dejar de arrastrar cuando se suelta el mouse
}
```

[Link App](https://editor.p5js.org/Majogc8/sketches/sJVhf0Irq)

![Image](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExdGhscXVmeTQ2anBhZXRoZzF4cXBqYmE3dXZ6eWh3cm9rNWxrazhiZCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/mlmwqPZfhCxD4vg18h/giphy.gif)
