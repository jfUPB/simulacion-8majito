### ¿Qué sucede cuando muchos objetos atraen a muchos objetos?
- Tecer ley de Newton, todas las fuerzas ocurren en pares: si un atractor atrae a un motor, entonces ese motor también debería atraer al atractor ------ *MoverAttractorBody*
- Consiste en resolver el movimiento de un grupo de objetos que interactúan a través de fuerzas gravitatorias.
- Eliminar la clase Attractor y hacer que los Mover se atraigan entre sí.
- Modificar la clase Mover para que contenga la lógica de atracción entre cuerpos.
- Actualizar el bucle principal en draw() para que cada Mover aplique fuerzas a todos los demás.

#### Codigo

```js
let fireworks = []; //Lista que almacena todas las partículas de fuegos artificiales
let G = 0.2; //Constante gravitacional que se usa en la atracción entre partículas
let numExplosions = 4; // Número fijo de explosiones activas al mismo tiempo

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < numExplosions; i++) { //Lanza numExplosions explosiones iniciales.
    launchFirework();
  }
}

function draw() {
  background(0, 25);

  //Atracción Gravitacional
  for (let firework of fireworks) {
    for (let other of fireworks) {
      if (firework !== other) {
        firework.attract(other); // Cada partícula atrae a todas las demás
      }
    }
  }

 // Actualiza y dibuja partículas
  for (let i = fireworks.length - 1; i >= 0; i--) { // Se recorre el array en reversa para eliminar elementos sin problemas
    let firework = fireworks[i];
    firework.update(); // Actualizar posición y velocidad
    firework.show(); // Dibujar la partícula
    if (firework.lifespan <= 0) { // Si su vida llega a 0, se elimina
      fireworks.splice(i, 1);
    }
  }

  // Genera nuevas explosiones cada 30 frames (~0.5 segundos)
  if (frameCount % 30 === 0) { // Genera explosiones periódicamente para mantener 4 activas
    while (fireworks.length < numExplosions * 50) { // Mantener un número máximo de partículas activas
      launchFirework();
    }
  }
}

// Función para lanzar un nuevo fuego artificial
function launchFirework() {
  let x = random(width); // Posición aleatoria en el ancho de la pantalla
  let y = height; // Inicia desde la parte inferior
  let color = [random(150, 255), random(150, 255), random(150, 255)];
  
  for (let i = 0; i < 50; i++) { // Crear 50 partículas por explosión
    let angle = random(TWO_PI); // Dirección aleatoria en 360°
    let speed = random(2, 6); // Velocidad aleatoria
    let vx = cos(angle) * speed; // Componente X de la velocidad
    let vy = sin(angle) * speed - random(1, 3); // Componente Y de la velocidad con impulso inicial
    fireworks.push(new Firework(x, y, vx, vy, random(2, 5), color)); // Agregar la partícula a la lista
  }
}

// Clase que representa cada partícula del fuego artificial
class Firework {
  constructor(x, y, vx, vy, m, color) { // Posición aleatoria en el ancho de la pantalla
    this.pos = createVector(x, y); // Posición inicial
    this.vel = createVector(vx, vy); // Velocidad inicial
    this.acc = createVector(0, 0); // Aceleración inicial
    this.mass = m;
    this.r = sqrt(this.mass) * random(1.5, 3);  // Radio basado en la masa con variación aleatoria
    this.color = color;
    this.lifespan = 255; // Tiempo de vida antes de desvanecerse
  }

 // Aplica una fuerza a la partícula
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass); // F = ma → a = F/m
    this.acc.add(f); // Se suma la aceleración resultante
  }

// Atracción gravitacional entre partículas
  attract(firework) {
    let force = p5.Vector.sub(this.pos, firework.pos); // Vector de distancia entre partículas
    let distanceSq = constrain(force.magSq(), 50, 500); // Limitar distancia para evitar fuerzas extremas
    let strength = (G * (this.mass * firework.mass)) / distanceSq; // Ley de gravitación F = G * (m1 * m2) / d²
    force.setMag(strength); // Aplicar la magnitud de la fuerza calculada
    firework.applyForce(force); // Aplicar la fuerza a la otra partícula
  }

  // Actualizar la partícula
  update() {
    this.vel.add(this.acc); // Aplicar la aceleración a la velocidad
    this.pos.add(this.vel); // Aplicar la velocidad a la posición
    this.acc.set(0, 0);  // Resetear la aceleración para evitar acumulaciones
    this.lifespan -= random(2, 5); // Variación en el desvanecimiento
  }

// Dibujar la partícula
  show() {
    noStroke();
    fill(this.color[0], this.color[1], this.color[2], this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
    fill(255, this.lifespan / 2); // Halo brillante
    ellipse(this.pos.x, this.pos.y, this.r * 3);
  }
}
```

2. Codigo 2 avanzado
```js
// Clase de cada partícula del fuego artificial
class Firework {
  constructor(x, y, vx, vy, m, color) {
    this.pos = createVector(x, y);
    this.vel = createVector(vx, vy);
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * random(1.5, 3);
    this.color = color;
    this.lifespan = 255;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  applyFriction() {
    let friction = this.vel.copy();
    friction.mult(-0.02);
    this.applyForce(friction);
  }

  attract(firework) {
    let force = p5.Vector.sub(this.pos, firework.pos);
    let distanceSq = constrain(force.magSq(), 50, 500);
    
    let colorSimilarity = this.getColorSimilarity(firework.color); 
    let strength = (G * this.mass * firework.mass * colorSimilarity) / distanceSq;

    force.setMag(strength);
    firework.applyForce(force);
  }

  getColorSimilarity(otherColor) {
    let diffR = abs(this.color[0] - otherColor[0]);
    let diffG = abs(this.color[1] - otherColor[1]);
    let diffB = abs(this.color[2] - otherColor[2]);
    return 1 - (diffR + diffG + diffB) / 765;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.set(0, 0);
    this.lifespan -= random(2, 5);
  }

  show() {
    noStroke();
    fill(this.color[0], this.color[1], this.color[2], this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.r * 2);
    fill(255, this.lifespan / 2);
    ellipse(this.pos.x, this.pos.y, this.r * 3);
  }
}
let fireworks = [];
let G = 0.2; // Constante gravitacional
let numExplosions = 4; // Número de explosiones activas
let windStrength = 0.02; // Intensidad del viento

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < numExplosions; i++) {
    launchFirework();
  }
}

function draw() {
  background(0, 25);

  // Aplicar atracción entre partículas
  for (let firework of fireworks) {
    for (let other of fireworks) {
      if (firework !== other) {
        firework.attract(other);
      }
    }
  }

  // Aplicar viento y fricción
  let wind = createVector(windStrength, 0);
  for (let firework of fireworks) {
    firework.applyForce(wind);
    firework.applyFriction();
    firework.update();
    firework.show();
  }

  // Eliminar partículas que han desaparecido
  fireworks = fireworks.filter(firework => firework.lifespan > 0);

  // Generar nuevas explosiones
  if (frameCount % 30 === 0) {
    while (fireworks.length < numExplosions * 50) {
      launchFirework();
    }
  }
}

// Nueva función para lanzar fuegos artificiales en diferentes alturas
function launchFirework() {
  let x = random(width);
  let y = random() < 0.5 ? height : random(height * 0.2, height * 0.5); 
  let color = [random(150, 255), random(150, 255), random(150, 255)];

  for (let i = 0; i < 50; i++) {
    let angle = random(TWO_PI);
    let speed = random(2, 6);
    let vx = cos(angle) * speed;
    let vy = sin(angle) * speed - random(1, 3);
    fireworks.push(new Firework(x, y, vx, vy, random(2, 5), color));
  }
}
```
#### Problema de los n-cuerpos

Este código simula fuegos artificiales que explotan en el cielo y se atraen mutuamente debido a una fuerza gravitacional simulada. La simulación representa un problema de n-cuerpos, porque cada partícula del fuego artificial afecta a todas las demás, generando interacciones complejas y dinámicas en el movimiento de los fragmentos de la explosión.

El problema de los n-cuerpos se refiere al estudio del movimiento de múltiples objetos que interactúan entre sí mediante fuerzas (como la gravedad). En este caso, cada fragmento de fuego artificial experimenta atracción gravitacional hacia todos los demás fragmentos en la simulación. Como resultado, la trayectoria de cada partícula cambia en función de la presencia y posición de las demás, creando patrones caóticos y realistas.

[Ejemplo 3](https://editor.p5js.org/Majogc8/sketches/Oagj9ZWsP)
