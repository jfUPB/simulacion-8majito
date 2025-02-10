let particles = [];
let numParticles = 300;
let starryNight;

function preload() {
  starryNight = loadImage('starry_night.jpg'); // Asegúrate de que la imagen esté en la carpeta del proyecto
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < numParticles; i++) {
    particles.push(new Particle(random(width), random(height)));
  }
}

function draw() {
  background(20, 20, 50);
  image(starryNight, 0, 0, width, height); // Dibuja la imagen de fondo
  
  for (let p of particles) {
    p.update();
    p.display();
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.acceleration = createVector(0, 0);
    this.maxSpeed = random(2, 5);
    this.baseColor = color(255, 204, 0); // Amarillo estrella
  }

  update() {
    let mouse = createVector(mouseX, mouseY);
    this.acceleration = p5.Vector.sub(mouse, this.position);
    this.acceleration.setMag(0.05); // Controla la fuerza de atracción
    
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
  }

  display() {
    let distance = dist(this.position.x, this.position.y, mouseX, mouseY);
    let c = lerpColor(this.baseColor, color(0, 153, 255), map(distance, 0, width, 0, 1));
    fill(c);
    noStroke();
    ellipse(this.position.x, this.position.y, 8, 8);
  }
}
