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

Intneto mil 
```js
let waves = [];
let numWaves = 5;  // Reducido de 8 a 5 ondas
let numPoints;
let time = 0;
let particles = [];
let maxParticles = 100;  // Limitar el número máximo de partículas

// Variables para el audio
let song;
let fft;
let amplitude;
let loaded = false;
let isPlaying = false;

// Bandas de frecuencia para analizar
let bass, lowMid, mid, highMid, treble;

// Variables para controles de usuario
let volumeSensitivity = 1.0;
let colorPalette = 0;
let showParticles = true;  // Opción para desactivar partículas
let quality = 1;  // Factor de calidad: 1=normal, 2=baja (menos puntos)

// Cargar el archivo de audio antes de iniciar
function preload() {
  soundFormats('mp3');
  song = loadSound('Piano.mp3',
    function () {
      loaded = true;
      console.log('Audio cargado correctamente');
    },
    function (err) {
      console.error('Error al cargar el audio:', err);
    }
  );
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  numPoints = floor(width / (10 * quality));  // Menos puntos en calidad baja

  // Configuración del análisis de audio
  fft = new p5.FFT(0.8, 512);  // Reducido de 1024 a 512 bins
  amplitude = new p5.Amplitude();

  // Inicializar ondas
  initializeWaves();
  noFill();

  // Agregar interfaz
  createPlayButton();
  createControlsInfo();
  createPerformanceControls();
}

function createPerformanceControls() {
  // Botón para alternar partículas
  let particlesBtn = createButton('Partículas: ON');
  particlesBtn.position(20, 60);
  particlesBtn.mousePressed(function() {
    showParticles = !showParticles;
    particlesBtn.html(showParticles ? 'Partículas: ON' : 'Partículas: OFF');
  });
  particlesBtn.class('play-button');
  
  // Botón para cambiar calidad
  let qualityBtn = createButton('Calidad: Alta');
  qualityBtn.position(20, 100);
  qualityBtn.mousePressed(function() {
    quality = quality === 1 ? 2 : 1;
    qualityBtn.html(quality === 1 ? 'Calidad: Alta' : 'Calidad: Baja');
    numPoints = floor(width / (10 * quality));
    initializeWaves();
  });
  qualityBtn.class('play-button');
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

  // Estilo básico para el botón
  let styles = `
    .play-button {
      padding: 10px 20px;
      background-color: rgba(255, 200, 100, 0.7);
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.3s ease;
    }
    .play-button:hover {
      background-color: rgba(255, 220, 150, 0.9);
    }
    .controls-info {
      position: fixed;
      bottom: 20px;
      left: 20px;
      color: rgba(255, 255, 255, 0.7);
      font-family: Arial, sans-serif;
      font-size: 14px;
      background-color: rgba(0, 0, 0, 0.5);
      padding: 10px;
      border-radius: 5px;
    }
  `;

  let styleEl = document.createElement('style');
  styleEl.textContent = styles;
  document.head.appendChild(styleEl);
}

function createControlsInfo() {
  let infoDiv = createDiv(
    '🎮 Controles:<br>' +
    '• Mouse X: Cambiar paleta de colores<br>' +
    '• Flechas ↑↓: Ajustar sensibilidad al volumen<br>' +
    '• Tecla R: Reiniciar visualización'
  );
  infoDiv.class('controls-info');
}

function togglePlay() {
  if (!loaded) {
    console.log('El audio aún no se ha cargado');
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

  // Actualizar la paleta de colores según mouseX
  updateColorPalette();

  // Análisis del audio si está reproduciéndose
  if (isPlaying) {
    analyzeAudio();
  }

  // Actualizar ondas
  for (let w of waves) {
    w.update();
  }

  // Dibujar capas suaves debajo de cada onda para volumen y sensación de tela
  // Solo dibujar una capa para mejorar rendimiento
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

  // Dibujar líneas de las ondas
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

  // Manejar partículas solo si están activadas
  if (showParticles) {
    // Emitir partículas desde las ondas (menos frecuentemente)
    if (frameCount % 3 === 0) {  // Reducido de cada frame a cada 3 frames
      for (let w of waves) {
        w.emitParticles(particles);
      }
    }

    // Limitar partículas para evitar sobrecarga
    while (particles.length > maxParticles) {
      particles.shift();  // Eliminar las más antiguas
    }

    // Agregar destellos con menos frecuencia
    if (isPlaying && frameCount % 10 === 0) {  // Menos frecuente
      let x = random(width);
      let y = random(height / 2, height);
      particles.push(new SparkleParticle(x, y));
    } else if (!isPlaying && frameCount % 15 === 0) {  // Menos frecuente
      let x = random(width);
      let y = random(height / 2, height);
      particles.push(new SparkleParticle(x, y));
    }

    // Actualizar y mostrar partículas
    for (let i = particles.length - 1; i >= 0; i--) {
      particles[i].followWave?.();
      particles[i].update();
      
      // Reducir uso de sombras (causa mayor impacto en rendimiento)
      if (frameCount % 2 === 0 && particles[i].isSparkle) {  // Solo aplicar a destellos y cada 2 frames
        drawingContext.shadowBlur = 10;  // Valor reducido
        drawingContext.shadowColor = 'rgba(255,180,80,0.8)';
      }
      
      particles[i].show();
      
      if (frameCount % 2 === 0 && particles[i].isSparkle) {
        drawingContext.shadowBlur = 0;
        drawingContext.shadowColor = 'transparent';
      }

      if (particles[i].isOffScreen()) {
        particles.splice(i, 1);
      }
    }
  }

  time += 0.02;

  // Mostrar información
  displayInfo();
}

// Mostrar toda la información
function displayInfo() {
  fill(255, 255, 255, 150);
  noStroke();
  textSize(14);
  textAlign(RIGHT);
  text(`Sensibilidad: ${volumeSensitivity.toFixed(1)}`, width - 20, 40);
  text(`Paleta: ${colorPalette + 1}/3`, width - 20, 60);
  text(`FPS: ${floor(frameRate())}`, width - 20, 80);
  text(`Partículas: ${particles.length}/${maxParticles}`, width - 20, 100);
}

// Actualizar paleta de colores basada en la posición del mouse
function updateColorPalette() {
  // Dividir la pantalla en 3 zonas de color
  colorPalette = floor(map(mouseX, 0, width, 0, 3));

  // Actualizar colores de ondas según la paleta seleccionada
  for (let i = 0; i < waves.length; i++) {
    let w = waves[i];

    switch (colorPalette) {
      case 0: // Paleta cálida (naranja/rojo)
        w.color1 = color(255, 200 - i * 40, 100 - i * 20, 180);
        w.color2 = color(255, 120 - i * 30, 80 - i * 10, 100);
        break;
      case 1: // Paleta fría (azul/violeta)
        w.color1 = color(100 - i * 20, 150 - i * 20, 255 - i * 10, 180);
        w.color2 = color(80 - i * 10, 100 - i * 10, 240 - i * 20, 100);
        break;
      case 2: // Paleta verde/amarillo
        w.color1 = color(150 + i * 20, 255 - i * 20, 100 - i * 10, 180);
        w.color2 = color(100 + i * 10, 200 - i * 20, 50 - i * 10, 100);
        break;
    }
  }
}

// Función para manejar teclas
function keyPressed() {
  // Ajustar sensibilidad con flechas arriba/abajo
  if (keyCode === UP_ARROW) {
    volumeSensitivity = constrain(volumeSensitivity + 0.1, 0.1, 3.0);
  } else if (keyCode === DOWN_ARROW) {
    volumeSensitivity = constrain(volumeSensitivity - 0.1, 0.1, 3.0);
  }

  // Reiniciar visualización con tecla R
  if (key === 'r' || key === 'R') {
    particles = [];
    time = 0;
    initializeWaves();
  }
}

// Función para analizar el audio y actualizar las variables globales
function analyzeAudio() {
  fft.analyze();

  // Obtener valores de diferentes rangos de frecuencia (0-1)
  bass = fft.getEnergy("bass") / 255;
  lowMid = fft.getEnergy("lowMid") / 255;
  mid = fft.getEnergy("mid") / 255;
  highMid = fft.getEnergy("highMid") / 255;
  treble = fft.getEnergy("treble") / 255;

  // Volumen general
  let level = amplitude.getLevel();

  // Ajustar la velocidad del tiempo basado en el nivel de audio y sensibilidad
  time += 0.01 + level * 0.03 * volumeSensitivity;
}

class Wave {
  constructor(color1, color2, yOffset, amplitude, noiseAmp) {
    this.color1 = color1;
    this.color2 = color2;
    this.yOffset = yOffset;
    this.amplitude = amplitude;
    this.noiseAmp = noiseAmp;
    this.points = [];
    this.responseFactor = random(0.5, 1.5);
  }

  update() {
    this.points = [];

    // Usar audio para afectar las ondas si está reproduciéndose
    let audioFactor = 1;
    let breathingSpeed = 0.5;
    let waveSpeed = 0.8;

    if (isPlaying) {
      // Las ondas superiores responden más a agudos, las inferiores a graves
      let yPosition = map(this.yOffset, height / 2 - 120, height / 2 + 480, 0, 1);

      // Las ondas más altas responden a frecuencias más altas
      let freqResponse = map(yPosition, 0, 1, bass + lowMid, mid + highMid + treble);

      // Ajustar amplitud basada en la energía de frecuencia correspondiente y sensibilidad
      audioFactor = 1 + freqResponse * 2 * this.responseFactor * volumeSensitivity;

      // Velocidad de "respiración" basada en ritmo bajo
      breathingSpeed = 0.5 + bass * 0.5 * volumeSensitivity;

      // Velocidad de movimiento ondulatorio basada en frecuencias medias
      waveSpeed = 0.8 + mid * 0.8 * volumeSensitivity;
    }

    let breathing = sin(time * breathingSpeed + this.yOffset) * 10 * audioFactor;
    let step = 10 * quality;  // Paso más grande en calidad baja

    for (let x = 0; x <= width; x += step) {
      let y = this.yOffset + breathing +
        sin(x * 0.01 + time * waveSpeed + this.yOffset * 0.002) * this.amplitude * audioFactor +
        noise(x * 0.003 + this.yOffset * 0.005, time * 0.1 + this.yOffset * 0.002) * this.noiseAmp;
      this.points.push({ x, y });
    }
  }

  getPointAndAngleAt(x) {
    let step = 10 * quality;
    let idx = floor(x / step);
    idx = constrain(idx, 0, this.points.length - 2);
    let p1 = this.points[idx];
    let p2 = this.points[idx + 1];
    let inter = map(x, p1.x, p2.x, 0, 1);
    let y = lerp(p1.y, p2.y, inter);
    let angle = atan2(p2.y - p1.y, p2.x - p1.x);
    return { y, angle };
  }

  emitParticles(particleArray) {
    // Menos partículas en general y especialmente en calidad baja
    let emissionFreq = isPlaying ? 15 : 20;
    
    if (frameCount % emissionFreq === 0) {
      let particleCount = isPlaying ? 1 : 1;
      
      // Emitir partículas con menos frecuencia
      for (let i = 0; i < this.points.length; i += 40) {  // Reducido de 20 a 40
        if (i < this.points.length) {
          let p = this.points[i];
          particleArray.push(new WaveParticle(p.x, p.y, this, true));
        }
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

    // La velocidad de las partículas puede ser afectada por el audio
    this.baseSpeed = random(0.5, 1.5);
    this.speed = this.baseSpeed;
    this.isSparkle = false;
  }

  followWave() {
    // Ajustar velocidad basada en la música si está reproduciéndose
    if (isPlaying) {
      this.speed = this.baseSpeed * (1 + mid * 1.5 * volumeSensitivity);
    } else {
      this.speed = this.baseSpeed;
    }

    let pos = this.wave.getPointAndAngleAt(this.x);
    this.y = lerp(this.y, pos.y, 0.1);
    let dir = this.floatsUp ? -1 : 1;
    this.x += cos(pos.angle) * this.speed;
    this.y += sin(pos.angle) * this.speed * dir;

    // Decaimiento más rápido para evitar acumulación
    let alphaDecay = 2.5;
    this.alpha -= alphaDecay;
  }

  update() {
    this.x += random(-0.1, 0.1);
    this.y += random(-0.1, 0.1);
    this.alpha -= 1.5;  // Más rápido
  }

  show() {
    noStroke();

    // Color básico según paleta
    let particleR, particleG, particleB;
    
    switch (colorPalette) {
      case 0: // Paleta cálida
        particleR = 255;
        particleG = 200;
        particleB = 150;
        break;
      case 1: // Paleta fría
        particleR = 150;
        particleG = 180;
        particleB = 255;
        break;
      case 2: // Paleta verde/amarillo
        particleR = 200;
        particleG = 255;
        particleB = 150;
        break;
    }

    fill(particleR, particleG, particleB, this.alpha * 0.6);
    ellipse(this.x, this.y, this.size);
  }

  isOffScreen() {
    return (this.x < -10 || this.x > width + 10 || this.y < -10 || this.y > height + 10 || this.alpha <= 0);
  }
}

// Clase para destellos flotantes brillantes (simplificada)
class SparkleParticle {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = isPlaying ? random(2, 4) : random(2, 4);
    this.alpha = random(100, 200);
    this.isSparkle = true;
    this.life = random(20, 40);  // Vida más corta
    this.maxLife = this.life;

    // Color base del destello según la paleta (simplificado)
    switch (colorPalette) {
      case 0: // Cálida
        this.colorR = 255;
        this.colorG = 220;
        this.colorB = 150;
        break;
      case 1: // Fría
        this.colorR = 150;
        this.colorG = 200;
        this.colorB = 255;
        break;
      case 2: // Verde/amarillo
        this.colorR = 220;
        this.colorG = 255;
        this.colorB = 150;
        break;
    }
  }

  update() {
    this.alpha = map(this.life, 0, this.maxLife, 0, 200);
    this.life -= 2;  // Decrementar más rápido
    
    // Movimiento simplificado
    this.x += random(-0.2, 0.2);
    this.y += random(-0.2, 0.2);
  }

  show() {
    noStroke();
    fill(this.colorR, this.colorG, this.colorB, this.alpha);
    ellipse(this.x, this.y, this.size);
  }

  isOffScreen() {
    return this.life <= 0 || this.alpha <= 0;
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  numPoints = floor(width / (10 * quality));
  initializeWaves();
}
```
