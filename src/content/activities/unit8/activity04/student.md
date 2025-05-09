#### Aplicación 
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Je te laisserai des mots - Visualización</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/addons/p5.sound.min.js"></script>
  <style>
    body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background-color: #030314;
      touch-action: none; /* Prevenir comportamientos táctiles por defecto */
    }
    #instructions {
      position: absolute;
      bottom: 20px;
      left: 20px;
      color: rgba(255, 255, 255, 0.7);
      font-family: sans-serif;
      font-size: 14px;
      pointer-events: none;
      transition: opacity 1s;
    }
    #playControls {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: rgba(0, 0, 0, 0.7);
      padding: 20px;
      border-radius: 8px;
      text-align: center;
      z-index: 99;
    }
    #playControls button {
      padding: 12px 24px;
      margin: 10px;
      font-size: 16px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    #playControls button:hover {
      background-color: #45a049;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div id="playControls">
    <h2>Audio listo</h2>
    <p>Haz clic en reproducir para comenzar la visualización</p>
    <button id="startBtn">Reproducir</button>
  </div>
  
  <div id="instructions">
    Mueve el ratón: Cambia los colores | Click: Ajusta sensibilidad | Espacio: Pausa/Reanuda
  </div>
  
  <script>
    let song;
    let fft;
    let amplitude;
    let loaded = false;
    let particles = [];
    let waves = [];
    let baseColor;
    let accentColor;
    let bgColor;
    let sensitivity = 1.5;
    let instructions;
    let userStartAudio = false; // Flag para controlar inicio de audio por usuario
    
    // Reemplaza esta URL con la ruta a tu archivo de audio
    // No necesitamos la variable audioURL ya que cargaremos el archivo en preload
    
    function preload() {
      // Cargar directamente el archivo de audio
      soundFormats('mp3', 'ogg', 'wav');
      song = loadSound("Piano.mp3");
    }
    
    function setup() {
      let cnv = createCanvas(windowWidth, windowHeight);
      
      // Habilitar toque inicial para dispositivos móviles
      cnv.touchStarted(canvasPressed);
      
      // Inicializar analizadores de audio
      fft = new p5.FFT(0.8, 1024);
      amplitude = new p5.Amplitude();
      
      // Colores iniciales
      updateColors(random(360));
      
      // Crear ondas iniciales
      for (let i = 0; i < 3; i++) {
        waves.push(new Wave(i * 0.5));
      }
      
      instructions = select('#instructions');
      
      // Ocultar instrucciones después de 8 segundos
      setTimeout(() => {
        if (instructions) {
          instructions.style('opacity', '0');
        }
      }, 8000);
      
      // Configurar el botón de inicio
      setupControls();
      
      // Cargar el audio
      loadAudio();
      
      // Asegurar que p5.sound esté inicializado
      getAudioContext().suspend();
    }
    
    function loadAudio() {
      // Ya no necesitamos cargar el audio aquí porque lo hacemos en preload()
      // Solo configuramos el volumen y marcamos como cargado
      if (song) {
        song.setVolume(0.7);
        loaded = true;
      } else {
        console.error("El audio no se cargó correctamente en preload()");
        alert("No se pudo cargar el audio. Verifica la ruta del archivo.");
      }
    }
    
    function setupControls() {
      // Configurar botón de inicio en los controles de reproducción
      const startBtn = select('#startBtn');
      startBtn.mousePressed(startAudio);
    }
    
    function startAudio() {
      // Asegurar que el contexto de audio esté activado
      if (getAudioContext().state !== 'running') {
        getAudioContext().resume();
      }
      
      if (loaded && song && !song.isPlaying()) {
        song.play();
        userStartAudio = true;
        
        // Ocultar controles después de iniciar
        select('#playControls').addClass('hidden');
      }
    }
    
    function draw() {
      // Fondo con efecto de desvanecimiento suave
      background(bgColor[0], bgColor[1], bgColor[2], 15);
      
      if (loaded && song && song.isPlaying()) {
        // Analizar el audio
        fft.analyze();
        let bass = fft.getEnergy("bass");
        let mid = fft.getEnergy("mid");
        let treble = fft.getEnergy("treble");
        let highMid = fft.getEnergy("highMid");
        
        // Obtener nivel de amplitud
        let level = amplitude.getLevel();
        
        // Visualizar espectro de frecuencias sutilmente
        drawSpectrum();
        
        // Actualizar y dibujar ondas
        for (let wave of waves) {
          wave.update(level);
          wave.display();
        }
        
        // Crear partículas en frecuencias agudas con una probabilidad basada en el nivel
        if (treble > 200 * sensitivity || highMid > 210 * sensitivity) {
          let x = random(width);
          let y = random(height);
          let size = map(treble, 0, 255, 2, 6) * sensitivity;
          let lifespan = map(highMid, 0, 255, 80, 200);
          
          particles.push(new Particle(x, y, size, lifespan));
          
          // Crear algunas partículas adicionales cuando la música es intensa
          if (level > 0.6 && random() > 0.7) {
            let bonusX = random(width);
            let bonusY = random(height);
            particles.push(new Particle(bonusX, bonusY, size * 0.8, lifespan * 0.7));
          }
        }
        
        // Actualizar y mostrar partículas
        for (let i = particles.length - 1; i >= 0; i--) {
          particles[i].update(level);
          particles[i].display();
          
          if (particles[i].isDead()) {
            particles.splice(i, 1);
          }
        }
        
        // Mostrar información de la reproducción
        showPlaybackInfo();
      } else {
        // Mensaje cuando no está reproduciendo (solo si ya está cargado)
        if (loaded && song && !song.isPlaying() && userStartAudio) {
          push();
          fill(0, 0, 0, 180);
          rect(width/2 - 140, height/2 - 15, 280, 30, 5);
          fill(255, 200);
          textAlign(CENTER, CENTER);
          textSize(18);
          text("Pausado - Presiona espacio para continuar", width/2, height/2);
          pop();
        } else if (loaded && !userStartAudio) {
          // Solo mostrar mensaje de inicio si no hay controles visibles
          if (select('#playControls').hasClass('hidden')) {
            push();
            fill(0, 0, 0, 180);
            rect(width/2 - 100, height/2 - 15, 200, 30, 5);
            fill(255, 200);
            textAlign(CENTER, CENTER);
            textSize(18);
            text("Toca para reproducir", width/2, height/2);
            pop();
          }
        } else if (!loaded) {
          push();
          fill(0, 0, 0, 180);
          rect(width/2 - 100, height/2 - 15, 200, 30, 5);
          fill(255, 200);
          textAlign(CENTER, CENTER);
          textSize(18);
          text("Cargando audio...", width/2, height/2);
          pop();
        }
      }
    }
    
    function drawSpectrum() {
      try {
        // Obtener espectro de frecuencias
        let spectrum = fft.analyze();
        
        // Dibujar forma del espectro muy sutilmente en la parte inferior
        push();
        noStroke();
        fill(red(baseColor), green(baseColor), blue(baseColor), 50);
        
        beginShape();
        vertex(0, height);
        for (let i = 0; i < spectrum.length; i++) {
          let x = map(i, 0, spectrum.length, 0, width);
          let h = map(spectrum[i], 0, 255, 0, 100);
          vertex(x, height - h);
        }
        vertex(width, height);
        endShape(CLOSE);
        pop();
      } catch (e) {
        console.error("Error al dibujar el espectro:", e);
      }
    }
    
    function showPlaybackInfo() {
      // Mostrar tiempo de reproducción
      if (song && song.isPlaying()) {
        try {
          push();
          fill(255, 100);
          textAlign(LEFT, TOP);
          textSize(12);
          
          // Formatear tiempo como mm:ss
          let currentTime = song.currentTime();
          let minutes = Math.floor(currentTime / 60);
          let seconds = Math.floor(currentTime % 60);
          let timeDisplay = minutes + ":" + (seconds < 10 ? "0" + seconds : seconds);
          
          text(timeDisplay, 20, 20);
          pop();
        } catch (e) {
          console.error("Error al mostrar tiempo de reproducción:", e);
        }
      }
    }
    
    function canvasPressed() {
      // Función para manejar toques en el canvas
      if (loaded) {
        if (song.isPlaying()) {
          // Cambiar sensibilidad al tocar la pantalla
          sensitivity = (sensitivity === 1.5) ? 2.0 : (sensitivity === 2.0) ? 1.0 : 1.5;
          
          // Mostrar indicador temporal de sensibilidad
          showSensitivityIndicator();
        } else {
          // Primer toque inicia la canción
          startAudio();
        }
      }
      return false;
    }
    
    function mousePressed() {
      // No ejecutar este comportamiento si se hizo clic en controles de UI
      if (isClickingUI()) {
        return true;
      }
      
      if (loaded) {
        if (song.isPlaying()) {
          // Click cambia la sensibilidad
          sensitivity = (sensitivity === 1.5) ? 2.0 : (sensitivity === 2.0) ? 1.0 : 1.5;
          
          // Mostrar indicador temporal de sensibilidad
          showSensitivityIndicator();
        } else {
          // Primer click inicia la canción
          startAudio();
        }
      }
      return false;
    }
    
    function isClickingUI() {
      // Comprobar si se está haciendo clic en elementos de la interfaz
      let playControls = select('#playControls').elt;
      
      if (mouseX === 0 && mouseY === 0) {
        return true; // Probablemente un evento sintético, no un clic real
      }
      
      if (document.elementFromPoint(mouseX, mouseY) === playControls ||
          playControls.contains(document.elementFromPoint(mouseX, mouseY))) {
        return true;
      }
      
      return false;
    }
    
    // Manejar pulsaciones de teclas para control adicional
    function keyPressed() {
      if (keyCode === 32) { // Barra espaciadora
        if (loaded) {
          if (song.isPlaying()) {
            song.pause();
          } else {
            song.play();
          }
        }
      }
    }
    
    function showSensitivityIndicator() {
      // Mostrar indicador temporal de sensibilidad
      let sensitivityLevel;
      if (sensitivity === 1.0) sensitivityLevel = "Baja";
      else if (sensitivity === 1.5) sensitivityLevel = "Media";
      else sensitivityLevel = "Alta";
      
      push();
      fill(0, 0, 0, 180);
      rect(width/2 - 100, height/2 - 15, 200, 30, 5);
      fill(255, 200);
      textAlign(CENTER, CENTER);
      textSize(18);
      text("Sensibilidad: " + sensitivityLevel, width/2, height/2);
      pop();
    }
    
    function mouseMoved() {
      // El movimiento del ratón cambia los colores
      updateColors(map(mouseX, 0, width, 0, 360));
    }
    
    function updateColors(hue) {
      // Actualizar esquema de colores basado en el tono
      colorMode(HSB, 360, 100, 100, 100);
      baseColor = color(hue, 80, 90, 70);
      accentColor = color((hue + 180) % 360, 85, 95, 80);
      
      // Color de fondo con baja saturación para una atmósfera suave
      bgColor = [
        hue > 180 ? 210 : 230, 
        20, 
        10, 
        100
      ];
    }
    
    function windowResized() {
      resizeCanvas(windowWidth, windowHeight);
    }
    
    class Wave {
      constructor(offset) {
        this.offset = offset;
        this.yOffset = random(1000);
        this.speed = random(0.005, 0.02);
        this.amplitude = random(20, 60);
        this.wavelength = random(0.005, 0.02);
        this.points = [];
      }
      
      update(level) {
        this.amplitude = map(level, 0, 1, 20, 80);
        this.yOffset += this.speed;
      }
      
      display() {
        push();
        noFill();
        stroke(baseColor);
        strokeWeight(2);
        
        beginShape();
        for (let x = 0; x < width + 10; x += 10) {
          let y = height / 2 + 
                 sin(x * this.wavelength + this.yOffset) * this.amplitude + 
                 cos(x * this.wavelength * 0.8 + this.yOffset + this.offset) * this.amplitude * 0.5;
                 
          vertex(x, y);
          
          // Guardar puntos para partículas
          this.points[floor(x/10)] = {x: x, y: y};
        }
        endShape();
        pop();
      }
      
      getRandomPoint() {
        if (this.points.length > 0) {
          return this.points[floor(random(this.points.length))];
        }
        return {x: random(width), y: random(height)};
      }
    }
    
    class Particle {
      constructor(x, y, size, lifespan) {
        this.pos = createVector(x, y);
        this.vel = createVector(random(-0.5, 0.5), random(-0.5, 0.5));
        this.acc = createVector(0, 0);
        this.size = size;
        this.maxSize = size;
        this.lifespan = lifespan;
        this.maxLife = lifespan;
        this.angle = random(TWO_PI);
        this.rotSpeed = random(-0.05, 0.05);
      }
      
      update(level) {
        // Añadir un poco de movimiento aleatorio
        let randomForce = p5.Vector.random2D();
        randomForce.mult(0.05);
        this.acc.add(randomForce);
        
        // Aplicar una fuerza suave hacia el centro
        let center = createVector(width/2, height/2);
        let dir = p5.Vector.sub(center, this.pos);
        dir.normalize();
        dir.mult(0.02);
        this.acc.add(dir);
        
        // Responder al nivel de audio
        this.vel.add(this.acc);
        this.vel.mult(0.98 + level * 0.1);
        this.pos.add(this.vel);
        this.acc.mult(0);
        
        // Actualizar ángulo y vida
        this.angle += this.rotSpeed * (1 + level);
        this.lifespan -= 1 + random(0.5);
        
        // La partícula crece con el audio y luego disminuye con el tiempo
        this.size = map(this.lifespan, this.maxLife, 0, this.maxSize, 0) * (1 + level * 2);
      }
      
      display() {
        push();
        translate(this.pos.x, this.pos.y);
        rotate(this.angle);
        
        // Dibujar partícula con brillo
        noStroke();
        let alpha = map(this.lifespan, 0, this.maxLife, 0, 100);
        
        // Efecto de resplandor
        fill(red(accentColor), green(accentColor), blue(accentColor), alpha * 0.2);
        ellipse(0, 0, this.size * 3, this.size * 3);
        
        fill(red(accentColor), green(accentColor), blue(accentColor), alpha * 0.6);
        ellipse(0, 0, this.size * 1.5, this.size * 1.5);
        
        fill(red(accentColor), green(accentColor), blue(accentColor), alpha);
        ellipse(0, 0, this.size, this.size);
        
        pop();
      }
      
      isDead() {
        return this.lifespan <= 0;
        
      }
    }
  </script>
</body>
</html>
