Esta simulación representa un sistema donde múltiples objetos (Mover) son atraídos por un punto central (Attractor).

Cada objeto tiene masa, velocidad y aceleración, y responde a fuerzas externas.

#### 1. Identificación de Motion 101

El Motion 101 es un marco conceptual que describe el movimiento de objetos en términos de posición, velocidad y aceleración. La idea clave es que:

La aceleración afecta la velocidad.
La velocidad afecta la posición.

´´´js
update() {
  this.velocity.add(this.acceleration); // La aceleración modifica la velocidad
  this.position.add(this.velocity); // La velocidad modifica la posición
  this.acceleration.mult(0); // Se reinicia la aceleración después de cada actualización
}
´´´

#### 2. ¿Qué modificación hay que hacer para agregar fuerzas acumulativas?

En Motion 101 básico, la aceleración se establece directamente (por ejemplo, apuntando hacia el mouse). Sin embargo, si queremos agregar fuerzas acumulativas, en vez de reiniciar la aceleración cada cuadro, debemos permitir que las fuerzas se sumen con el tiempo.

La modificación clave está en cómo aplicamos la fuerza

´´´js
applyForce(force) {
  let f = p5.Vector.div(force, this.mass); // F = ma (fuerza dividida por masa)
  this.acceleration.add(f); // Se acumulan fuerzas en cada cuadro
}
´´´

#### 3. Identificar el Attractor en la simulación

- El Attractor es el objeto que genera la fuerza de atracción para los Mover. 

´´´js
let attractor;
attractor = new Attractor();
´´´

- En la función draw(), el Attractor interactúa con cada Mover:

js

attractor.display(); // Dibuja el atractor

for (let i = 0; i < movers.length; i++) {
  let force = attractor.attract(movers[i]); // Calcula la fuerza de atracción
  movers[i].applyForce(force); // Aplica la fuerza al objeto

  movers[i].update(); // Actualiza el movimiento del objeto
  movers[i].show(); // Dibuja el objeto
}
