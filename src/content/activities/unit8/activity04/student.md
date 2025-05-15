#### Configuración de p5.sound

*1. Carga del audio:*

function preload() {
  soundFormats('mp3');
  song = loadSound('Piano.mp3', () => { loaded = true; }, err => console.error('Error al cargar audio:', err));
}
- Uso loadSound para cargar el archivo Piano.mp3 antes de empezar.
- Marco loaded para saber cuándo el audio está listo.

*2. Inicialización en setup():*

fft = new p5.FFT(0.8, 512);       // Para análisis de frecuencias
amplitude = new p5.Amplitude();  // Para medir nivel general de volumen
- p5.FFT permite obtener espectro de frecuencias (bass, mid, treble).
- p5.Amplitude mide la amplitud (nivel global de volumen).

*3.Extracción de datos de audio y uso en el algoritmo*

function analyzeAudio() {
  fft.analyze();                       // Actualiza análisis de frecuencias
  bass = fft.getEnergy("bass");       // Extrae energía en banda baja (graves)
  mid = fft.getEnergy("mid");         // Extrae energía en banda media
  level = amplitude.getLevel();       // Obtiene nivel global de volumen

  for (let i = 0; i < waves.length; i++) {
    waves[i].amplitude = (40 + bass / 5 + i * 10) * amplitudeSensitivity;  // Modula la amplitud de las ondas con bass y sensibilidad
    waves[i].freqMod = map(mid, 0, 255, 1.5, 3.5);                         // Modula frecuencia con energía media
  }
}
- bass modula la amplitud vertical de cada ola para hacerlas más grandes cuando hay más graves.
- mid modula la frecuencia (número de oscilaciones) para hacer las olas más rápidas o lentas.
- level (amplitud general) controla la cantidad y velocidad de partículas y destellos.

*4. Uso de datos de audio para controlar outputs visuales*

Olas que varían en tamaño y frecuencia:

for (let w of waves) {
  w.update();  // Calcula puntos usando amplitude y freqMod que vienen del audio
}
- Partículas emitidas según nivel de volumen:

let particlesToEmit = floor(level * 15 * volumeSensitivity);
for (let i = 0; i < particlesToEmit; i++) {
  let w = random(waves);
  w.emitParticles(particles);
}
- Destellos arriba que aumentan con volumen:

let sparkleCount = floor(level * 100 * volumeSensitivity);
for (let i = 0; i < sparkleCount; i++) {
  if (random() < 0.3) {
    topSparkles.push(new TopSparkle(random(width), random(0, height * 0.2), level));
  }
}

Resumen
- p5.sound da análisis espectral y amplitud.

- Use bandas específicas (bass, mid) para modular la forma y comportamiento de las ondas.

- Use el nivel general (level) para modular la cantidad e intensidad de partículas y destellos.

- Todo esto genera una visualización fluida, dinámica y sensible a la música en tiempo real.

#### App

[Porfin](https://editor.p5js.org/Majogc8/sketches/6B9XpuRjH)

[!Image](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExYnk5NG5vMGZ2ZHgzOG4yaTh6cWxja2g5cXl2MnNuZWFmNm56Ymc0cCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ojviaIszkF0yX9WI4E/giphy.gif)
