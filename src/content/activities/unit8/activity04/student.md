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
let waves = [];
let numWaves = 8;
let numPoints;
let time = 0;
let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  numPoints = width / 10;

  for (let i = 0; i < numWaves; i++) {
    let c1 = color(255, 200 - i * 40, 100 - i * 20, 180);
    let c2 = color(255, 120 - i * 30, 80 - i * 10, 100);

    let yOffset = height / 2 + (i - 1.5) * 80;
    let amplitude = 40 + i * 15;
    let noiseFactor = 50 - i * 8;

    waves.push(new Wave(c1, c2, yOffset, amplitude, noiseFactor));
  }

  noFill();
}

function draw() {
  background(0, 30);
  blendMode(BLEND);

  // Actualizar ondas
  for (let w of waves) {
    w.update();
  }

  // Dibujar capas suaves debajo de cada onda para volumen y sensación de tela
  noStroke();
  for (let w of waves) {
    for (let layer = 0; layer < 3; layer++) {
      fill(red(w.color1), green(w.color1), blue(w.color1), 15 - layer * 5);
      beginShape();
      vertex(0, height);
      for (let p of w.points) {
        vertex(p.x, p.y + 15 + layer * 5);
      }
      vertex(width, height);
      endShape(CLOSE);
    }
  }

  // Dibujar líneas de las ondas con curvas suaves y gradientes alpha variable
  strokeWeight(2);
  for (let w of waves) {
    noFill();
    beginShape();
    for (let i = 0; i < w.points.length; i++) {
      let p = w.points[i];
      let interC = lerpColor(w.color1, w.color2, map(i, 0, w.points.length - 1, 0, 1));
      stroke(red(interC), green(interC), blue(interC), 150);
      curveVertex(p.x, p.y);
    }
    endShape();
  }

  // Emitir partículas desde las ondas
  for (let w of waves) {
    w.emitParticles(particles);
  }

  // Agregar partículas extra tipo destello dispersas aleatoriamente (destellos "flotantes")
  if (frameCount % 5 === 0) {
    let x = random(width);
    let y = random(height / 2, height);
    particles.push(new SparkleParticle(x, y));
  }

  // Actualizar y mostrar partículas con glow y movimiento suave
  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].followWave?.();
    particles[i].update();

    // Para destellos y partículas normales, brillo pulsante
    drawingContext.shadowBlur = particles[i].isSparkle ? 20 + 10 * sin(time * 20 + i) : 12;
    drawingContext.shadowColor = particles[i].isSparkle ? 'rgba(255, 220, 120, 0.9)' : 'rgba(255,180,80,0.8)';

    particles[i].show();

    drawingContext.shadowBlur = 0;
    drawingContext.shadowColor = 'transparent';

    if (particles[i].isOffScreen()) {
      particles.splice(i, 1);
    }
  }

  time += 0.02;
}

class Wave {
  constructor(color1, color2, yOffset, amplitude, noiseAmp) {
    this.color1 = color1;
    this.color2 = color2;
    this.yOffset = yOffset;
    this.amplitude = amplitude;
    this.noiseAmp = noiseAmp;
    this.points = [];
  }

  update() {
    this.points = [];
    let breathing = sin(time * 0.5 + this.yOffset) * 10;
    for (let x = 0; x <= width; x += 10) {
      let y = this.yOffset + breathing +
                sin(x * 0.01 + time * 0.8 + this.yOffset * 0.002) * this.amplitude +
                noise(x * 0.003 + this.yOffset * 0.005, time * 0.1 + this.yOffset * 0.002) * this.noiseAmp;
      this.points.push({ x, y });
    }
  }

  getPointAndAngleAt(x) {
    let idx = floor(x / 10);
    idx = constrain(idx, 0, this.points.length - 2);
    let p1 = this.points[idx];
    let p2 = this.points[idx + 1];
    let inter = map(x, p1.x, p2.x, 0, 1);
    let y = lerp(p1.y, p2.y, inter);
    let angle = atan2(p2.y - p1.y, p2.x - p1.x);
    return { y, angle };
  }

  emitParticles(particleArray) {
    let freq = 5;
    if (frameCount % freq === 0) {
      for (let i = 0; i < this.points.length; i += 20) {
        let p = this.points[i];
        particleArray.push(new WaveParticle(p.x, p.y, this, true));
        particleArray.push(new WaveParticle(p.x + random(-5, 5), p.y, this, false));
      }
    }
  }
}

class WaveParticle {
  constructor(x, y, wave, floatsUp) {
    this.x = x;
    this.y = y;
    this.wave = wave;
    this.floatsUp = floatsUp;
    this.size = random(1, 3);
    this.alpha = 255;
    this.speed = random(0.5, 1.5);
    this.isSparkle = false;
  }

  followWave() {
    let pos = this.wave.getPointAndAngleAt(this.x);
    this.y = lerp(this.y, pos.y, 0.1);
    let dir = this.floatsUp ? -1 : 1;
    this.x += cos(pos.angle) * this.speed;
    this.y += sin(pos.angle) * this.speed * dir;
    this.alpha -= 1.5;
  }

  update() {
    this.x += random(-0.1, 0.1);
    this.y += random(-0.1, 0.1);
    this.alpha -= 0.7;
  }

  show() {
    noStroke();
    fill(255, 200, 150, this.alpha * 0.6);
    ellipse(this.x, this.y, this.size * 0.7);
  }

  isOffScreen() {
    return (this.x < -10 || this.x > width + 10 || this.y < -50 || this.y > height + 50 || this.alpha <= 0);
  }
}

// Nueva clase para destellos flotantes brillantes
class SparkleParticle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = random(2, 5);
    this.alpha = random(100, 255);
    this.isSparkle = true;
    this.life = random(40, 80);
    this.maxLife = this.life;
  }

  update() {
    this.alpha = map(this.life, 0, this.maxLife, 0, 255);
    this.life--;
    // Pequeño movimiento aleatorio
    this.x += random(-0.2, 0.2);
    this.y += random(-0.2, 0.2);
  }

  show() {
    noStroke();
    // Brillo pulsante
    let glowSize = this.size + 3 * sin(time * 20);
    fill(255, 255, 200, this.alpha);
    ellipse(this.x, this.y, glowSize);
    fill(255, 255, 220, this.alpha);
    ellipse(this.x, this.y, glowSize * 0.5);
  }

  isOffScreen() {
    return this.life <= 0 || this.alpha <= 0;
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```

**Intneto mil**
```js
let waves = [];
let numWaves = 5;  // Número fijo de ondas
let numPoints;
let time = 0;
let particles = [];
let maxParticles = 100;

// Variables para el audio
let song;
let fft;
let amplitude;
let loaded = false;
let isPlaying = false;

// Bandas de frecuencia para analizar
let bass, mid;

// Variables fijas
let volumeSensitivity = 1.0;
let colorPalette = 0;
let quality = 1;  // Calidad fija alta (sin cambio)

// Cargar audio antes de iniciar
function preload() {
  soundFormats('mp3');
  song = loadSound('Piano.mp3',
    function () {
      loaded = true;
    },
    function (err) {
      console.error('Error al cargar el audio:', err);
    }
  );
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  numPoints = floor(width / (10 * quality));

  fft = new p5.FFT(0.8, 512);
  amplitude = new p5.Amplitude();

  initializeWaves();
  noFill();

  createPlayButton();
}

function initializeWaves() {
  waves = [];
  for (let i = 0; i < numWaves; i++) {
    let c1 = color(255, 200 - i * 40, 100 - i * 20, 180);
    let c2 = color(255, 120 - i * 30, 80 - i * 10, 100);

    let yOffset = height / 2 + (i - 1.5) * 80;
    let amplitude = 40 + i * 15;
    let noiseFactor = 50 - i * 8;

    waves.push(new Wave(c1, c2, yOffset, amplitude, noiseFactor));
  }
}

function createPlayButton() {
  let playBtn = createButton('▶️ Play');
  playBtn.position(20, 20);
  playBtn.mousePressed(togglePlay);
  playBtn.class('play-button');

  // Estilos mínimos para botón
  let styles = `
    .play-button {
      padding: 10px 20px;
      background-color: rgba(255, 200, 100, 0.7);
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s ease;
      z-index: 10;
      position: fixed;
    }
    .play-button:hover {
      background-color: rgba(255, 220, 150, 0.9);
    }
  `;

  let styleEl = document.createElement('style');
  styleEl.textContent = styles;
  document.head.appendChild(styleEl);
}

function togglePlay() {
  if (!loaded) {
    return;
  }

  if (isPlaying) {
    song.pause();
    select('.play-button').html('▶️ Play');
  } else {
    song.play();
    select('.play-button').html('⏸️ Pause');
  }

  isPlaying = !isPlaying;
}

function draw() {
  background(0, 30);
  blendMode(BLEND);

  updateColorPalette();

  if (isPlaying) {
    analyzeAudio();
  }

  for (let w of waves) {
    w.update();
  }

  noStroke();
  for (let w of waves) {
    fill(red(w.color1), green(w.color1), blue(w.color1), 15);
    beginShape();
    vertex(0, height);
    for (let p of w.points) {
      vertex(p.x, p.y + 15);
    }
    vertex(width, height);
    endShape(CLOSE);
  }

  strokeWeight(2);
  for (let w of waves) {
    noFill();
    beginShape();
    for (let i = 0; i < w.points.length; i++) {
      let p = w.points[i];
      let interC = lerpColor(w.color1, w.color2, map(i, 0, w.points.length - 1, 0, 1));
      stroke(red(interC), green(interC), blue(interC), 150);
      curveVertex(p.x, p.y);
    }
    endShape();
  }

  // Emitir partículas siempre
  if (frameCount % 3 === 0) {
    for (let w of waves) {
      w.emitParticles(particles);
    }
  }

  // Limitar partículas para rendimiento
  while (particles.length > maxParticles) {
    particles.shift();
  }

  if (frameCount % 10 === 0) {
    let x = random(width);
    let y = random(height / 2, height);
    particles.push(new SparkleParticle(x, y));
  }

  for (let i = particles.length - 1; i >= 0; i--) {
    particles[i].followWave?.();
    particles[i].update();
    particles[i].show();

    if (particles[i].isOffScreen()) {
      particles.splice(i, 1);
    }
  }

  time += 0.02;
}

// Actualiza la paleta de colores según el mouseX
function updateColorPalette() {
  let zone = floor(map(mouseX, 0, width, 0, 3));
  if (zone !== colorPalette) {
    colorPalette = zone;
    for (let i = 0; i < waves.length; i++) {
      let baseHue = 30 + i * 40 + colorPalette * 60;
      waves[i].color1 = color(baseHue, 200, 200, 180);
      waves[i].color2 = color(baseHue + 20, 150, 150, 100);
    }
  }
}

// Análisis de audio: amplitud con bass y frecuencia con mid
function analyzeAudio() {
  fft.analyze();
  bass = fft.getEnergy("bass");
  mid = fft.getEnergy("mid");

  for (let i = 0; i < waves.length; i++) {
    waves[i].amplitude = 40 + bass / 5 + i * 10;
    waves[i].freqMod = map(mid, 0, 255, 1.5, 3.5);
  }
}

// Clase Wave
class Wave {
  constructor(color1, color2, yOffset, amplitude, noiseFactor) {
    this.color1 = color1;
    this.color2 = color2;
    this.yOffset = yOffset;
    this.amplitude = amplitude;
    this.noiseFactor = noiseFactor;
    this.points = [];
    this.freqMod = 2; // frecuencia base para oscilación
  }

  update() {
    this.points = [];
    for (let i = 0; i <= numPoints; i++) {
      let x = map(i, 0, numPoints, 0, width);
      let n = noise(i * 0.1, time);
      let y = this.yOffset + this.amplitude * sin(TWO_PI * i / numPoints * this.freqMod + time * 2) + this.noiseFactor * n;
      this.points.push({ x, y });
    }
  }

  emitParticles(particlesArray) {
    if (random() < 0.3) {
      let index = floor(random(this.points.length));
      let p = this.points[index];
      particlesArray.push(new WaveParticle(p.x, p.y, this));
    }
  }
}

// Partículas que siguen las ondas
class WaveParticle {
  constructor(x, y, wave) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.3, 1));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.wave = wave;
    this.size = random(3, 6);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 3;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  followWave() {
    let closestX = constrain(this.pos.x, 0, width);
    let i = floor(map(closestX, 0, width, 0, this.wave.points.length - 1));
    let targetY = this.wave.points[i].y + 15;
    let forceY = (targetY - this.pos.y) * 0.02;
    this.applyForce(createVector(0, forceY));
  }

  show() {
    noStroke();
    fill(255, 150, 100, this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isOffScreen() {
    return this.lifespan <= 0 || this.pos.x < 0 || this.pos.x > width || this.pos.y > height || this.pos.y < 0;
  }
}

// Partículas brillantes aleatorias
class SparkleParticle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.5, 2));
    this.lifespan = 200;
    this.size = random(4, 8);
    this.isSparkle = true;
  }

  update() {
    this.pos.add(this.vel);
    this.lifespan -= 6;
  }

  show() {
    noStroke();
    fill(255, 180, 80, this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isOffScreen() {
    return this.lifespan <= 0;
  }
}
```
