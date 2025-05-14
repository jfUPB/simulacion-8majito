```js
let song;
let fft;
let amplitude;
let particles = [];
let ribbons = [];
let sparkles = [];
let soundWaves = []; // Nueva array para las ondas sonoras
let baseColor, accentColor;
let lastLevel = 0;
let levelHistory = [];
let maxLevelHistory = 50;
let colorPalette = [];
let numWaves = 8; // Número de ondas sonoras en la pantalla

function preload() {
  // Cargar directamente el archivo de audio
  soundFormats('mp3', 'ogg', 'wav');
  song = loadSound("Piano.mp3");
}

// Clase para los destellos de fondo como en la imagen 2
class Sparkle {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.size = random(1, 4);
    this.alpha = random(100, 255);
    this.blinkRate = random(0.01, 0.05);
    this.offset = random(0, 360);
  }
  
  update(level) {
    // Parpadeo suave
    this.alpha = 150 + 105 * sin(frameCount * this.blinkRate + this.offset);
    
    // Responder ligeramente al audio
    this.size = map(level, 0, 1, 1, 4) * random(0.8, 1.2);
  }
  
  display() {
    // Dibujar como pequeño destello estelar
    noStroke();
    fill(45, 10, 100, this.alpha);
    ellipse(this.x, this.y, this.size);
    
    // Rayos de luz ocasionales
    if (random() > 0.99) {
      stroke(45, 10, 100, this.alpha * 0.5);
      strokeWeight(0.5);
      let len = random(3, 8);
      line(this.x - len, this.y, this.x + len, this.y);
      line(this.x, this.y - len, this.x, this.y + len);
    }
  }
}

// Nueva clase para las ondas de sonido que cubren toda la pantalla
class SoundWave {
  constructor() {
    this.yBase = random(height); // Posición base vertical
    this.waveSpeed = random(0.5, 2); // Velocidad de movimiento horizontal
    this.frequency = random(0.01, 0.03); // Frecuencia de onda
    this.amplitude = random(20, 50); // Amplitud base
    this.thickness = random(2, 6); // Grosor de la línea
    this.color = colorPalette[floor(random(colorPalette.length))];
    this.phase = random(360); // Fase inicial
    this.frequencyBand = floor(random(10, 200)); // Banda de frecuencia específica a la que responde
    this.alpha = random(40, 90); // Transparencia
    this.yMovement = random(-0.5, 0.5); // Velocidad vertical lenta
  }
  
  update(spectrum, level) {
    // La onda se mueve horizontalmente
    this.phase += this.waveSpeed;
    
    // Lento movimiento vertical
    this.yBase += this.yMovement;
    
    // Mantener dentro de los límites de la pantalla con rebote suave
    if (this.yBase < 0 || this.yBase > height) {
      this.yMovement *= -1;
      this.yBase = constrain(this.yBase, 0, height);
    }
    
    // Ajustar la amplitud según la banda de frecuencia específica
    let freqValue = spectrum[this.frequencyBand];
    this.currentAmplitude = this.amplitude * (1 + map(freqValue, 0, 255, 0, 2));
    
    // Aumentar el grosor según el nivel general de audio
    this.currentThickness = this.thickness * (1 + level * 2);
  }
  
  display() {
    push();
    noFill();
    stroke(hue(this.color), saturation(this.color), brightness(this.color), this.alpha);
    strokeWeight(this.currentThickness);
    
    beginShape();
    for (let x = 0; x <= width; x += 10) {
      // Crear una onda sinusoidal con variación de amplitud
      let xOffset = (x * this.frequency) + this.phase;
      let y = this.yBase + sin(xOffset) * this.currentAmplitude;
      
      vertex(x, y);
    }
    endShape();
    pop();
  }
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  angleMode(DEGREES);
  colorMode(HSB, 360, 100, 100, 100);
  
  // Generar paleta de colores inspirada en la primera imagen
  colorPalette = [
    color(0, 80, 100),    // Rojo
    color(20, 80, 100),   // Naranja
    color(45, 80, 100),   // Amarillo dorado
    color(180, 80, 100),  // Cian
    color(220, 80, 90),   // Azul
    color(270, 70, 90),   // Violeta
  ];
  
  // Colores base para la visualización
  baseColor = color(220, 80, 90);    // Azul para ondas principales
  accentColor = color(45, 100, 100); // Dorado para destellos
  
  // Configurar analizadores de audio
  fft = new p5.FFT(0.9, 256);
  amplitude = new p5.Amplitude();
  
  song.loop(); // Reproduce en bucle
  fft.setInput(song);
  amplitude.setInput(song);
  
  // Crear cintas iniciales con más densidad
  for (let i = 0; i < 8; i++) {
    ribbons.push(new Ribbon());
  }
  
  // Crear ondas sonoras distribuidas por la pantalla
  for (let i = 0; i < numWaves; i++) {
    soundWaves.push(new SoundWave());
  }
  
  // Crear destellos iniciales
  for (let i = 0; i < 100; i++) {
    sparkles.push(new Sparkle());
  }
  
  // Fondo azul profundo como en la segunda imagen
  background(220, 90, 20);
}

function draw() {
  // Crear fondo con desvanecimiento suave para efecto de rastro
  background(220, 90, 20, 5); // Fondo azul profundo con baja opacidad para crear estelas
  
  // Obtener datos de audio
  let spectrum = fft.analyze();
  let level = amplitude.getLevel();
  levelHistory.push(level);
  
  // Mantener el historial de amplitud en un tamaño fijo
  if (levelHistory.length > maxLevelHistory) {
    levelHistory.shift();
  }
  
  // Detectar picos en la amplitud para eventos especiales
  let isPeak = false;
  if (level > lastLevel * 1.2 && level > 0.2) {
    isPeak = true;
    // Añadir nuevas partículas en eventos de pico (destellos brillantes)
    for (let i = 0; i < 25; i++) {
      particles.push(new Particle());
    }
    // Ocasionalmente añadir nuevas cintas
    if (random() > 0.6 && ribbons.length < 12) {
      ribbons.push(new Ribbon());
    }
    // Añadir ocasionalmente una nueva onda
    if (random() > 0.8 && soundWaves.length < numWaves + 5) {
      soundWaves.push(new SoundWave());
    }
  }
  lastLevel = level;
  
  // Actualizar y mostrar destellos de fondo (como en la segunda imagen)
  for (let i = 0; i < sparkles.length; i++) {
    sparkles[i].update(level);
    sparkles[i].display();
  }
  
  // Dibujar las ondas sonoras por toda la pantalla
  for (let i = 0; i < soundWaves.length; i++) {
    soundWaves[i].update(spectrum, level);
    soundWaves[i].display();
  }
  
  // Centro brillante como en la segunda imagen (cuando hay picos de sonido)
  let centerBrightness = map(level, 0, 1, 0, 100);
  if (centerBrightness > 15) {
    let centerRadius = map(level, 0, 1, 50, 200);
    for (let i = 10; i > 0; i--) {
      let alpha = map(i, 0, 10, 0, 90);
      fill(45, 20, 100, alpha);
      ellipse(width/2, height/2, centerRadius * i / 5);
    }
  }
  
  // Actualizar y mostrar cintas ondulantes (como en la primera imagen)
  for (let i = ribbons.length - 1; i >= 0; i--) {
    ribbons[i].update(spectrum, level);
    ribbons[i].display();
    
    // Eliminar cintas antiguas
    if (ribbons[i].lifespan <= 0) {
      ribbons.splice(i, 1);
    }
  }
  
  // Actualizar y mostrar partículas brillantes
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].update(level);
    particles[i].display();
    
    // Eliminar partículas antiguas
    if (particles[i].lifespan <= 0) {
      particles.splice(i, 1);
    }
  }
  
  // Dibujar forma de onda en la parte inferior (opcional)
  drawWaveform();
}

function drawWaveform() {
  let waveform = fft.waveform();
  noFill();
  strokeWeight(2);
  
  // Dibujar la forma de onda en la parte inferior
  beginShape();
  for (let i = 0; i < waveform.length; i++) {
    let x = map(i, 0, waveform.length, 0, width);
    let y = map(waveform[i], -1, 1, height - 100, height - 20);
    
    // Color basado en la posición y valor de la onda
    let hue = map(i, 0, waveform.length, 180, 240);
    let sat = map(abs(waveform[i]), 0, 1, 40, 100);
    stroke(hue, sat, 90, 30);
    
    vertex(x, y);
  }
  endShape();
}

function drawAmplitudeHistory() {
  noFill();
  strokeWeight(3);
  stroke(40, 100, 100, 50);
  
  beginShape();
  for (let i = 0; i < levelHistory.length; i++) {
    let x = map(i, 0, maxLevelHistory, 0, width);
    let y = map(levelHistory[i], 0, 1, height * 0.75, height * 0.25);
    vertex(x, y);
  }
  endShape();
}

// Clase para las cintas ondulantes multicolores como en la primera imagen
class Ribbon {
  constructor() {
    this.points = [];
    this.maxPoints = 150;
    this.lifespan = 350;
    this.colorIndex = floor(random(colorPalette.length));
    this.color = colorPalette[this.colorIndex];
    this.nextColorIndex = (this.colorIndex + 1) % colorPalette.length;
    this.nextColor = colorPalette[this.nextColorIndex];
    this.x = random(width);
    this.y = random(height);
    this.baseWidth = random(30, 80);
    this.frequencyBand = floor(random(20, 150)); // Banda de frecuencia a la que responde
    this.yOffset = random(-300, 300); // Para posicionar en diferentes alturas
    this.xSpeed = random(0.5, 1.5); // Velocidad horizontal
    this.waveAmplitude = random(20, 80); // Amplitud de la onda
    this.waveFrequency = random(0.01, 0.05); // Frecuencia de la onda
  }
  
  update(spectrum, level) {
    // Actualizar la posición basada en la música
    let freq = spectrum[this.frequencyBand];
    let mappedFreq = map(freq, 0, 255, 0, 1);
    
    // Movimiento fluido horizontal constante
    this.x += this.xSpeed;
    
    // Movimiento ondulatorio vertical más suave y controlado
    this.y = height/2 + this.yOffset + 
             sin(this.x * this.waveFrequency) * this.waveAmplitude * (1 + level * 2);
    
    // Si la cinta sale del canvas, reiniciar desde la izquierda
    if (this.x > width + 100) {
      this.x = -100;
      this.colorIndex = floor(random(colorPalette.length));
      this.color = colorPalette[this.colorIndex];
      this.nextColorIndex = (this.colorIndex + 1) % colorPalette.length;
      this.nextColor = colorPalette[this.nextColorIndex];
    }
    
    // Añadir un nuevo punto a la cinta
    this.points.push({
      x: this.x,
      y: this.y,
      width: this.baseWidth * (0.8 + mappedFreq * 0.5),
      alpha: map(mappedFreq, 0, 1, 50, 90),
      progress: this.points.length / this.maxPoints
    });
    
    // Limitar el número de puntos
    if (this.points.length > this.maxPoints) {
      this.points.shift();
    }
    
    // Reducir la vida útil más lentamente
    this.lifespan -= 0.3;
  }
  
  display() {
    if (this.points.length < 2) return;
    
    // Dibujar la cinta como una forma continua suave
    noStroke();
    
    // Dibujar la cinta con gradientes de color suaves
    for (let i = 0; i < this.points.length - 1; i++) {
      let p1 = this.points[i];
      let p2 = this.points[i + 1];
      
      // Calcular vectores perpendiculares para el ancho de la cinta
      let angle = atan2(p2.y - p1.y, p2.x - p1.x);
      let perpAngle = angle + 90;
      
      let x1 = p1.x + cos(perpAngle) * p1.width / 2;
      let y1 = p1.y + sin(perpAngle) * p1.width / 2;
      let x2 = p1.x - cos(perpAngle) * p1.width / 2;
      let y2 = p1.y - sin(perpAngle) * p1.width / 2;
      
      let x3 = p2.x - cos(perpAngle) * p2.width / 2;
      let y3 = p2.y - sin(perpAngle) * p2.width / 2;
      let x4 = p2.x + cos(perpAngle) * p2.width / 2;
      let y4 = p2.y + sin(perpAngle) * p2.width / 2;
      
      // Interpolación de color a lo largo de la cinta y entre colores adyacentes
      let progressAlongRibbon = map(i, 0, this.points.length - 1, 0, 1);
      let c1, c2;
      
      // Interpolación entre colores de la paleta para un efecto de arcoíris como en imagen 1
      let colorProgress = (progressAlongRibbon + frameCount * 0.001) % 1;
      let hue1 = map(colorProgress, 0, 1, hue(this.color), hue(this.nextColor));
      let hue2 = map(colorProgress + 0.01, 0, 1, hue(this.color), hue(this.nextColor));
      
      // Crear colores con saturación y brillo altos para imitar la imagen de referencia
      c1 = color(hue1, 80, 95, p1.alpha);
      c2 = color(hue2, 80, 95, p1.alpha);

      // Dibujar el segmento como un quad con gradiente
      beginShape();
      fill(c1);
      vertex(x1, y1);
      vertex(x2, y2);
      fill(c2);
      vertex(x3, y3);
      vertex(x4, y4);
      endShape(CLOSE);
    }
  }
}

// Clase para partículas que aparecen en picos de amplitud
class Particle {
  constructor() {
    this.x = random(width);
    this.y = random(height);
    this.size = random(3, 15);
    this.speed = random(1, 3);
    this.angle = random(360);
    this.lifespan = 255;
    this.hue = random(0, 360);
  }
  
  update(level) {
    // Movimiento basado en ruido de Perlin para suavidad
    let noiseScale = 0.01;
    let noiseVal = noise(this.x * noiseScale, this.y * noiseScale, frameCount * 0.01);
    
    this.angle += map(noiseVal, 0, 1, -5, 5);
    
    // La velocidad se ve afectada por el nivel de audio
    let speedMod = map(level, 0, 1, 0.5, 2);
    this.x += cos(this.angle) * this.speed * speedMod;
    this.y += sin(this.angle) * this.speed * speedMod;
    
    // Reducir la vida útil
    this.lifespan -= 2;
  }
  
  display() {
    // Efecto de brillo
    noStroke();
    for (let i = 3; i > 0; i--) {
      let alpha = map(i, 0, 3, 0, this.lifespan);
      fill(this.hue, 80, 100, alpha / 255 * 30);
      ellipse(this.x, this.y, this.size * i * 1.5);
    }
    
    // Centro de la partícula
    fill(this.hue, 30, 100, this.lifespan);
    ellipse(this.x, this.y, this.size);
  }
}

function mousePressed() {
  // Toggle reproducción/pausa al hacer clic
  if (song.isPlaying()) {
    song.pause();
  } else {
    song.play();
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```

Ondas 
```js
let song;
let fft;
let phase = 0; // Fase inicial para la onda
let amplitude = 100; // Amplitud de la onda
let frequency = 0.01; // Frecuencia de la onda
let speed = 0.05; // Velocidad de la fase para animar la onda

function setup() {
  createCanvas(400, 400);
  song = loadSound('Piano.mp3', startSound); // Asegúrate de tener el archivo 'song.mp3'
  fft = new p5.FFT();
  stroke(255);
  noFill();
}

function startSound() {
  song.play();
}

function draw() {
  background(0);

  // Obtener la forma de onda del sonido
  let waveform = fft.waveform();

  // Dibujar la onda senoidal
  beginShape();
  for (let x = 0; x < width; x++) {
    // Modificar la forma de la onda dependiendo de la música
    let y = height / 2 + waveform[x % waveform.length] * amplitude; // Usar la onda del sonido
    vertex(x, y);
  }
  endShape();

  // Animar la onda senoidal con la fase
  phase += speed; // Cambiar la fase para mover la onda
}
```js
