### Idea

#### Intención de Diseño

El objetivo de esta aplicación es crear una versión interactiva de La Noche Estrellada de Vincent van Gogh, utilizando un sistema de partículas que se comportan como pinceladas en movimiento. Las partículas reaccionarán a la interacción del usuario y se desplazarán siguiendo patrones similares a los remolinos de la pintura original.

#### ¿Cómo piensas aplicar el marco MOTION 101 y por qué estos?

Aplicaré MOTION 101 modelando cada partícula con los tres componentes clave:

- Posición: Determina dónde está cada partícula en el lienzo.
- Velocidad: Define la dirección y rapidez con la que se mueve la partícula.
- Aceleración: En este caso, usaré aceleración hacia el mouse, lo que hará que las partículas se vean atraídas por el cursor, generando una sensación de movimiento fluido e interactivo.
  
#### Reporta los referentes que usaste para inspirarte.
- La Noche Estrellada de Vincent van Gogh (por sus colores y trazos dinámicos)
-  Patrik Hübner


### Cambio de idea:

#### Intención de Diseño

Mi intención con esta simulación es representar un iris en movimiento dinámico, evocando la sensación de un ojo vivo y cambiante. Utilizando partículas dispuestas en patrones concéntricos, el objetivo es crear un efecto hipnótico y envolvente, donde la aceleración y el color aporten fluidez y profundidad a la imagen.

#### ¿Cómo piensas aplicar el marco MOTION 101 y por qué estos?

Aplicaré MOTION 101 modelando cada partícula con los tres componentes clave:

- Posición: Cada partícula sigue una trayectoria circular alrededor del centro del iris, manteniendo un patrón estructurado.
- Velocidad: Se asigna una velocidad angular individual para dar la sensación de movimiento independiente en cada partícula.
- Aceleración: Implementé una aceleración dependiente del movimiento del mouse. Si el usuario mueve el mouse a la derecha, el iris gira más rápido; si lo mueve a la izquierda, el giro se ralentiza. Esto introduce un control interactivo y dinámico en la simulación.

Además, aplico lerp() para suavizar los cambios en la distancia radial de las partículas, dando un efecto más orgánico, y lerpColor() para crear transiciones suaves en los colores del iris, reforzando la estética viva y natural.

#### Reporta los referentes que usaste para inspirarte.
- El iris humano y sus patrones: Observé imágenes macro de ojos humanos para entender cómo los patrones de pigmentación crean estructuras radiales.
- Van Gogh y el movimiento en el arte: La fluidez y el uso del color en Starry Night inspiraron la forma en que las partículas se mueven y cambian de color.
- Simulaciones de partículas interactivas: Exploré proyectos en p5.js y Processing donde las partículas responden a la entrada del usuario, lo que me llevó a implementar la aceleración según el movimiento del mouse.
-  Patrik Hübner
