#### 1. Diseña el proceso generativo

**¿Cuál será la lógica central que genere los visuales?**

- Ondas orgánicas que se mueven suavemente en el fondo, como una tela o superficie líquida. Se generan con una malla de puntos deformada por ruido Perlin, cuya intensidad cambia con el volumen (amplitud) de la música.

- Partículas brillantes que aparecen durante los momentos de alta energía en las frecuencias agudas (como la parte mas rapida). Las partículas flotan suavemente hacia arriba, con trayectorias afectadas por ruido y una ligera fuerza de dispersión.

- Destellos suaves que surgen aleatoriamente, influenciados por el análisis de frecuencia completo (FFT). Se usan para dar una sensación de "respirar" junto con la música.

Esta lógica busca mantener una visualización fluida, dinámica y siempre cambiante, en línea con el piano emocional y orgánico de Je te laisserai des mots.

#### 2. Elige conceptos/técnicas de simulación o generación

**Ruido Perlin deformando una malla de ondas**

Crearé una red de puntos que se moverá suavemente como olas.

- El ruido Perlin se usará para animar las posiciones en y de los puntos, generando ese movimiento ondulante natural.
- Este ruido estará modulado por la amplitud de la música: a mayor volumen, más intensidad en la deformación.

**Sistema de partículas luminosas**

Se generarán partículas que nacen en momentos de alta energía en las frecuencias agudas (FFT).

Estas partículas tendrán:

- Movimiento suave hacia arriba.
- Variación de tamaño y velocidad.
- Opacidad que disminuye con el tiempo.

**Aleatoriedad controlada + fuerzas suaves**

Las trayectorias de las partículas usarán:
- Fuerza suave hacia arriba.
- Desviaciones con ruido Perlin (ligeros desvíos horizontales).

#### 3. Diseño documentado del algoritmo generativo

**Inputs**

Amplitud (volumen):
- Modula la intensidad de las ondas en el fondo.
- Cambia el tamaño y brillo de las partículas.

FFT (análisis de frecuencias):
- Agudos (2000–8000 Hz): disparan partículas brillantes.
- Medios: afectan color y cantidad de destellos suaves.

Interacción del usuario:
- mouseX: cambia la paleta de colores.
- flechas ↑↓: modifican la sensibilidad al volumen.
- Tecla r: reinicia la visualización.

**Proceso generativo**

Fondo ondulante (Olas)
- Se crea una malla 2D de puntos.
- Cada punto se mueve en y usando ruido Perlin:
```js
y = baseY + noise(x, tiempo) * amplitud * factor
La amplitud de la música modifica el factor.
```

Partículas brillantes

En cada frame, si FFT.agudos > threshold, se generan partículas:
- Posición inicial: zona inferior.
- Velocidad: vertical hacia arriba.
- Desviación horizontal con Perlin o ruido simple.
- Desvanecimiento de opacidad.

Destellos suaves

Basado en energía media (FFT), se generan destellos:

- Se colocan aleatoriamente en la escena.
- Escala y transparencia varían suavemente.
- Se usan como respiraciones visuales del piano.

***Pseudocódigo clave***
```js
// Fondo
for (let x = 0; x < cols; x++) {
  for (let y = 0; y < rows; y++) {
    let yOffset = noise(x * escala, y * escala, tiempo);
    puntos[y][x].pos.y = baseY + yOffset * amplitud * 100;
  }
}

// FFT agudos genera partículas
if (fft.getEnergy(6000) > umbral) {
  generarParticula(random(ancho), altura);
}

// Actualizar partículas
for (let p of particulas) {
  p.vel.y -= gravedad;
  p.vel.x += noise(p.pos.x, tiempo) * 0.5;
  p.update();
  p.display();
}
```

![image](https://github.com/user-attachments/assets/2827e39c-ab90-4624-9a2e-61e9cab1292e)
