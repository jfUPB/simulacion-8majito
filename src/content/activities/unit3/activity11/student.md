### ¿Qué sucede cuando muchos objetos atraen a muchos objetos?
- Tecer ley de Newton, todas las fuerzas ocurren en pares: si un atractor atrae a un motor, entonces ese motor también debería atraer al atractor ------ *MoverAttractorBody*
- Consiste en resolver el movimiento de un grupo de objetos que interactúan a través de fuerzas gravitatorias.
- Eliminar la clase Attractor y hacer que los Mover se atraigan entre sí.
- Modificar la clase Mover para que contenga la lógica de atracción entre cuerpos.
- Actualizar el bucle principal en draw() para que cada Mover aplique fuerzas a todos los demás.

#### Codigo

```js
// Clase Firework que representa cada partícula de los fuegos artificiales
class Firework {
  constructor(x, y, vx, vy, m, color) { // Motion 101
    this.pos = createVector(x, y); // Posición inicial
    this.vel = createVector(vx, vy); // Velocidad inicial
    this.acc = createVector(0, 0); // Aceleración inicial
    this.mass = m; // Masa de la partícula
    this.r = sqrt(this.mass) * random(1.5, 3); // Radio basado en la masa
    this.color = color; 
    this.lifespan = 255; // Duración de la partícula
  }

  // Aplicar una fuerza externa considerando la masa
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass); // F = ma -> a = F/m
    this.acc.add(f); // Sumar aceleración
  }

  // Aplicar fricción para reducir la velocidad gradualmente
  applyFriction() {
    let friction = this.vel.copy(); // Copiar velocidad actual
    friction.mult(-0.02); // Aplicar fricción en dirección opuesta
    this.applyForce(friction);
  }

  // Atracción entre partículas basada en la Ley de Gravitación Universal, modificada por color
  attract(firework) {
    let force = p5.Vector.sub(this.pos, firework.pos); // Vector de atracción
    let distanceSq = constrain(force.magSq(), 50, 500); // Limitar distancia mínima y máxima
    let colorSimilarity = this.getColorSimilarity(firework.color); // Factor de atracción basado en color
    let strength = (G * this.mass * firework.mass * colorSimilarity) / distanceSq; // Fuerza de atracción
    force.setMag(strength); // Ajustar magnitud
    firework.applyForce(force); // Aplicar la fuerza
  }

  // Calcular similitud entre colores (cuanto más parecidos, mayor atracción)
  getColorSimilarity(otherColor) {
    let diffR = abs(this.color[0] - otherColor[0]);
    let diffG = abs(this.color[1] - otherColor[1]);
    let diffB = abs(this.color[2] - otherColor[2]);
    return 1 - (diffR + diffG + diffB) / 765; // Normalización entre 0 y 1
  }

  // Actualizar posición y disminuir vida útil
  update() {
    this.vel.add(this.acc); // Aplicar aceleración a la velocidad
    this.pos.add(this.vel); // Aplicar velocidad a la posición
    this.acc.set(0, 0); // Reiniciar aceleración para el siguiente frame
    this.lifespan -= random(2, 5); // Reducir vida útil
  }

  // Dibujar la partícula en pantalla
  show() {
    noStroke();
    fill(this.color[0], this.color[1], this.color[2], this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
    fill(255, this.lifespan / 2);
    ellipse(this.pos.x, this.pos.y, this.r * 3);
  }
}

// Lanzar un fuego artificial en la posición del clic
function mousePressed() {
  launchFirework(mouseX, mouseY);
}

// Controles de teclado para modificar parámetros
function keyPressed() {
  if (key === 'A' || key === 'a') {
    numExplosions++; // Aumentar cantidad de explosiones
  } else if (key === 'D' || key === 'd') {
    numExplosions = max(1, numExplosions - 1); // Disminuir cantidad de explosiones
  } else if (key === 'W' || key === 'w') {
    windStrength += 0.01; // Aumentar viento
  } else if (key === 'S' || key === 's') {
    windStrength = max(0, windStrength - 0.01); // Reducir viento
  }
}

// Definición de variables globales
let fireworks = []; // Array que almacena las partículas de los fuegos artificiales
let G = 0.2; // Constante gravitacional usada para la atracción entre partículas
let numExplosions = 4; // Número de explosiones activas
let windStrength = 0.02; // Intensidad del viento

function setup() {
  createCanvas(600, 400); // Crear el lienzo de 600x400 píxeles
  for (let i = 0; i < numExplosions; i++) {
    launchFirework(); // Generar explosiones iniciales
  }
}

function draw() {
  background(0, 25); // Fondo con transparencia para efecto de estela

  // Aplicar atracción entre partículas (Ley de gravitación modificada por color)
  for (let firework of fireworks) {
    for (let other of fireworks) {
      if (firework !== other) {
        firework.attract(other);
      }
    }
  }

  // Aplicar fuerzas externas como viento y fricción
  let wind = createVector(windStrength, 0); // Vector de viento en la dirección X
  for (let firework of fireworks) {
    firework.applyForce(wind); // Aplicar fuerza del viento
    firework.applyFriction(); // Aplicar fricción para disminuir velocidad
    firework.update(); // Actualizar la posición y velocidad
    firework.show(); // Dibujar la partícula
  }

  // Eliminar partículas que han desaparecido
  fireworks = fireworks.filter(firework => firework.lifespan > 0);

  // Generar nuevas explosiones periódicamente
  if (frameCount % 30 === 0) {
    while (fireworks.length < numExplosions * 50) { // Asegurar número de partículas por explosión
      launchFirework();
    }
  }

  // Mostrar controles en pantalla
  fill(255);
  textSize(12);
  textAlign(LEFT);
  text("A: Aumenta el número de explosiones", 10, height - 50);
  text("D: Disminuye el número de explosiones", 10, height - 35);
  text("W: Aumenta la fuerza del viento", 10, height - 20);
  text("S: Reduce la fuerza del viento", 10, height - 5);
}

// Función para lanzar fuegos artificiales con alturas controladas por Perlin Noise
function launchFirework(x = random(width), y = map(noise(frameCount * 0.1), 0, 1, height * 0.2, height * 0.5)) {
  let baseColor = [random(150, 255), random(150, 255), random(150, 255)]; // Color base aleatorio

  for (let i = 0; i < 50; i++) { // Crear múltiples partículas por explosión
    let angle = random(TWO_PI); // Ángulo aleatorio
    let speed = random(2, 6); // Velocidad inicial
    let vx = cos(angle) * speed; // Componente X de la velocidad
    let vy = sin(angle) * speed - random(1, 3); // Componente Y de la velocidad con un ligero impulso hacia arriba
    let color = [
      lerp(baseColor[0], random(255), 0.5),
      lerp(baseColor[1], random(255), 0.5),
      lerp(baseColor[2], random(255), 0.5)
    ];
    fireworks.push(new Firework(x, y, vx, vy, random(2, 5), color)); // Agregar nueva partícula
  }
}

```
#### Problema de los n-cuerpos

Este código simula fuegos artificiales que explotan en el cielo y se atraen mutuamente debido a una fuerza gravitacional simulada. La simulación representa un problema de n-cuerpos, porque cada partícula del fuego artificial afecta a todas las demás, generando interacciones complejas y dinámicas en el movimiento de los fragmentos de la explosión.

El problema de los n-cuerpos se refiere al estudio del movimiento de múltiples objetos que interactúan entre sí mediante fuerzas (como la gravedad). En este caso, cada fragmento de fuego artificial experimenta atracción gravitacional hacia todos los demás fragmentos en la simulación. Como resultado, la trayectoria de cada partícula cambia en función de la presencia y posición de las demás, creando patrones caóticos y realistas.

[Aplicación]([https://editor.p5js.org/Majogc8/sketches/Oagj9ZWsP](https://editor.p5js.org/Majogc8/sketches/cTpWS6sM1)
