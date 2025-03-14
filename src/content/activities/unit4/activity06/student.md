### Función Sinusoide :atom:

```js
let periodSlider, amplitudeSlider, phaseSlider, speedSlider;
let phase = 0;
let amplitude = 200;
let period = 120;
let speed = 0.02;

function setup() {
  createCanvas(800, 400);

  // Crear sliders para controlar parámetros
  periodSlider = createSlider(10, 300, 120, 1);
  periodSlider.position(20, height - 60);
  
  amplitudeSlider = createSlider(50, 300, 200, 1);
  amplitudeSlider.position(20, height - 40);
  
  phaseSlider = createSlider(0, TWO_PI, 0, 0.05);
  phaseSlider.position(20, height - 20);

  speedSlider = createSlider(0.01, 0.2, 0.02, 0.01);
  speedSlider.position(20, height - 80);
  
   // Etiquetas de los sliders
  textSize(14);
  fill(255);
}

function draw() {
  background(20);
  
  text("Speed", 160, height - 65);
  text("Period", 160, height - 45);
  text("Amplitude", 160, height - 25);
  text("Phase", 160, height - 5); 
  
  // Leer valores de sliders
  period = periodSlider.value();
  amplitude = amplitudeSlider.value();
  phase = phaseSlider.value();
  speed = speedSlider.value();

  let x1 = amplitude * sin((TWO_PI * frameCount * speed) / period);
  let x2 = amplitude * sin((TWO_PI * frameCount * speed) / period + phase);

  translate(width / 2, height / 2);

  // Líneas de referencia
  stroke(200);
  line(-amplitude, 0, amplitude, 0);
  
  // Dibujar círculos con colores brillantes
  let c1 = color(255, 0, 0);
  let c2 = color(0, 255, 255);
  let c3 = color(255, 255, 0);

  noStroke();
  fill(c1);
  circle(x1, 0, 48);

  fill(c2);
  circle(x2, 50, 48);

  fill(c3);
  circle(x1 - x2, 100, 48);
}
```
- Velocidad angular (speedSlider) → Controla qué tan rápido se mueve la onda.
- Período (periodSlider) → Define qué tan larga es la onda.
- Amplitud (amplitudeSlider) → Modifica qué tan grande es la oscilación.
- Fase (phaseSlider) → Cambia el desfase en tiempo real.

![image](../../../../assets/Attractor Morado.png)
[Sinusoide](https://editor.p5js.org/Majogc8/sketches/Yj53_TDMo)
