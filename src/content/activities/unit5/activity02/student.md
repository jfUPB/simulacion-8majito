#### Sistema de particulas ❇️

Un sistema de partículas es una colección de muchas, muchas partículas diminutas que juntas representan un objeto borroso. Durante un período de tiempo, las partículas se generan en un sistema, se mueven y cambian desde dentro del sistema, y mueren desde el sistema.

La matriz JavaScript, una estructura de datos que almacena una lista arbitrariamente larga de valores. En JavaScript, un array es en realidad un objeto creado a partir de la clase, por lo que viene con muchos métodos incorporados. Estos métodos proporcionan toda la funcionalidad que necesito para mantener una lista de objetos, incluida la adición de partículas, la eliminación de partículas o la manipulación de las mismas.

#### Example 4.2: An Array of Particles

**Creación y la desaparición de las partículas**
- *Creación:* Cada fotograma (draw()), se crea una nueva partícula y se añade al array particles. La posición inicial de la partícula está en el centro superior del lienzo (width / 2, 20).
- *Desaparición:* Se revisa el array de partículas desde el final hacia el principio con un bucle inverso. Si una partícula cumple la condición isDead() (es decir, su lifespan es menor que 0), se elimina del array usando - -splice(). Esto asegura que el array no crezca indefinidamente y que se libere memoria al eliminar partículas "muertas".

**Gestión de la memoria**  Al eliminar partículas "muertas" con splice(), se evita que el array acumule objetos innecesarios. Esto es fundamental para que la simulación sea eficiente y no ralentice el sistema por un exceso de partículas en memoria.

***Concepto aplicado:***

Se implementó el uso de aceleración hacia un punto específico (el mouse) como fuerza adicional. Este concepto proviene de la física básica: las partículas sienten una atracción hacia un objetivo, similar a la gravedad en un campo gravitatorio.

- *Justificación:* La aceleración hacia un punto permite agregar interactividad a la simulación, haciendo que las partículas respondan al movimiento del mouse.


[Simulación 1](https://editor.p5js.org/Majogc8/sketches/X4iFERsli)

![Image](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExb3RpaXh5cGsyNm81MDdmejRjdnhkdTdyM2IzamV2M3JybjRuanFhdiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/jsxdKIPFYQctmw62Ns/giphy.gif)

#### Example 4.4: A System of Systems

**Creación de partículas:** Cada emisor (clase Emitter) tiene su propio sistema de partículas (array particles), y se generan nuevas partículas con el método addParticle(). Este método se llama en cada iteración del draw().

**Desaparición de partículas:** Cada partícula tiene una propiedad lifespan que disminuye en cada ciclo hasta llegar a 0. Una vez que lifespan es menor que 0, el método isDead() devuelve true. Luego, en el método run() del emisor, las partículas muertas se eliminan del array particles mediante splice().

**Gestión de memoria:**
- La eliminación de partículas muertas reduce la carga de memoria, evitando que el sistema mantenga objetos innecesarios.
- El array emitters se actualiza dinámicamente al agregar nuevos emisores, pero no elimina ninguno durante la simulación.

***Conceptos aplicados 🧷***

1. *Velocidad inicial suavizada con lerp:*  Uso de interpolación lineal (lerp) para controlar la generación de velocidades iniciales.
- Cómo se aplicó: Se genera una velocidad inicial para las partículas que varía dentro de un rango, pero de manera más controlada y progresiva que si se usara simplemente random(-1, 1).
- Por qué se usó: Esto permite que las partículas tengan movimientos iniciales más suaves, lo que crea una estética más fluida y natural.

2. *Fuerza de atracción dinámica:* Aplicación de fuerzas mediante vectores y oscilación dinámica.
- Cómo se aplicó: Se crea un punto de atracción (attractor) que oscila horizontalmente con una función seno. Luego, se calcula una fuerza de atracción entre el punto y cada partícula usando la diferencia de vectores (p5.Vector.sub).
- Por qué se usó: Este movimiento dinámico añade complejidad al sistema, haciendo que las partículas sigan un movimiento fluido e impredecible. Esto imita sistemas naturales como partículas atraídas hacia un imán.

3. *Interpolación de colores (lerpColor):* Transición suave entre dos colores basada en la vida de las partículas.
- Cómo se aplicó: Se generan dos colores base (c1 y c2) y se interpolan linealmente usando lerpColor, dependiendo del porcentaje de vida restante (this.lifespan / 255).
- Por qué se usó: Esto crea un efecto visual más atractivo y dinámico, donde las partículas cambian de color a medida que se desvanecen.

4. *Acumulación de fuerzas:*  Uso de aceleración acumulada para definir el movimiento de las partículas, según el modelo de Motion 101.
- Cómo se aplicó: Se suman todas las fuerzas a la aceleración, y esta a su vez modifica la velocidad y posición. Cada paso reinicia la aceleración con this.acceleration.mult(0) para evitar acumulaciones no deseadas.
- Por qué se usó: Este modelo garantiza que las partículas tengan movimientos realistas que respeten las leyes físicas básicas.

5. *Movimiento oscilatorio (función seno):* Uso de funciones trigonométricas para crear oscilaciones en el movimiento.
- Cómo se aplicó:Se usa la función sin con un desplazamiento (timeOffset) para mover el punto de atracción de manera ondulatoria en el eje X.
- Por qué se usó: Este comportamiento ondulante añade un patrón visual interesante y dinámico, evitando movimientos estáticos y predecibles.

[Simulación 2](https://editor.p5js.org/Majogc8/sketches/CDJnfXDDC)

![Image](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTJ4YmZiam92Mng5ZDFscmxia3plenR5NXMyajhuZHRwZzN6b2U3ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Bwuufi466N804TCovv/giphy.gif)

#### Example 4.5: Un sistema de partículas con herencia y polimorfismo

**Creación:** En el método addParticle() de la clase Emitter, se genera aleatoriamente un nuevo objeto de la clase Particle o Confetti. Estos objetos son agregados al array this.particles. Esto asegura que constantemente se creen partículas mientras se ejecute el sistema.

**Actualización:** En el método run(), cada partícula del array se actualiza llamando a sus métodos update() y show(). Las partículas aplican fuerzas, cambian su posición y reducen su valor lifespan (duración de vida).

**Desaparición y limpieza:** Si la vida útil de una partícula (lifespan) llega a 0, se considera "muerta". Esto se verifica en el método isDead() de cada partícula. En el método run() del emisor, las partículas muertas son eliminadas del array this.particles usando splice(). Esto garantiza que la memoria no se sature con objetos inactivos.

***Conceptos aplicados 🧷***

1. *Ruido Perlin:* Se usó para generar movimientos suaves y orgánicos en las partículas, evitando cambios bruscos. Cada partícula tiene un desplazamiento único que afecta su trayectoria.
- Cómo se aplicó: direcciones de movimiento suavizadas usando un ángulo derivado del ruido. Esto dio fluidez a las trayectorias. 
- Por qué se usó:Para suavidad y naturalidad en el movimiento.(Aunque para mi esta super brusco)
  
2. *Lévy Flight:*  Introdujimos pasos cortos frecuentes y ocasionales pasos largos, simulando comportamientos naturales impredecibles.
- Cómo se aplicó: Calculamos tamaños de paso variables con una distribución inversa, limitándolos para evitar saltos extremos.
- Por qué se usó: Para variedad visual e impredecibilidad controlada.

3. *Gravedad:* Se añadió una fuerza constante hacia abajo para anclar las partículas al entorno.
- Cómo se aplicó:  Añadimos una fuerza constante hacia abajo para un comportamiento más realista.
- Por qué se usó: Para anclar las partículas al "mundo físico" y evitar movimientos incoherentes.

[Simulación 3](https://editor.p5js.org/Majogc8/sketches/_z2QypdSO)

![Image](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbXo1cTFkM256dzUyMWY5dDhrZ216NXc1OXkwdGt1Y2xqODI2M3gzciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/HitnscuKKx10lV10Nk/giphy.gif)

####  Example 4.6: Un sistema de partículas con fuerzas

**Creación:**
- Las partículas son creadas dentro del método addParticle() de la clase Emitter.
- Cada vez que addParticle() es llamado en el draw(), una nueva instancia de la clase Particle se genera y se agrega al arreglo this.particles.

**Desaparición y limpieza:**
- Cada partícula tiene una propiedad lifespan, que disminuye con cada frame (this.lifespan -= 2.0 en el método update() de la clase Particle).
- Cuando el valor de lifespan llega a cero (this.lifespan < 0.0), el método isDead() retorna true.
- En el método run() del emisor, se verifica si alguna partícula está "muerta". Si lo está, se elimina del arreglo this.particles usando el método splice().

***Conceptos aplicados 🧷***

1. *Caminatas aleatorias:* Las partículas realizan movimientos impredecibles utilizando el ruido Perlin.
- Cómo se aplicó: Se usó la función noise() para generar ángulos de movimiento suaves y pseudoaleatorios. 
- Por qué se usó: Para dar un movimiento orgánico y fluido a las partículas, aportando estética y naturalidad.
2. *Rebotes en los bordes:* Las partículas rebotan al llegar a los límites del canvas, invirtiendo su dirección.
- Cómo se aplicó: Al detectar que una partícula alcanza los bordes, la dirección de su velocidad en ese eje (x o y) se invierte multiplicándola por -1.
- Por qué se usó: Para mantener las partículas dentro del canvas, haciendo que la interacción con los bordes sea dinámica y visualmente atractiva.

[Simulación 4](https://editor.p5js.org/Majogc8/sketches/ic8KkVDuQ)

![Image](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExZmdiM3QxZ2RhNjFjcDVwejdycjN5emwycTgycnZ0MHo2MDhsYjAxZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/u7ajnJMDb2WITBY6z2/giphy.gif)

#### Exmaple 4.7: Un sistema de partículas con un repelente

**Creación:**
- En el método draw(), el sistema de partículas (Emitter) invoca addParticle().
- Este método agrega una nueva instancia de la clase Particle a la lista de partículas (this.particles).
- Cada nueva partícula se posiciona en el origen (this.origin) del Emitter, definido al momento de su creación.

**Desaparición y limpieza:**
- En cada frame, las partículas se actualizan mediante particle.update():
- Se reduce su lifespan en 2 unidades por frame.
- Si lifespan cae por debajo de 0, la partícula es considerada "muerta".
- En el método run() del Emitter, se revisa cada partícula con particle.isDead():
  - Si devuelve true, esa partícula se elimina de la lista con this.particles.splice(i, 1).

**Gestión de memoria:**
- Las partículas se almacenan en el array this.particles del Emitter.
- Cada vez que una partícula "muere", se elimina del array con splice(), liberando esa referencia de memoria.
- Esto garantiza que no se acumulen partículas innecesarias en el sistema, optimizando el uso de memoria.


***Conceptos aplicados 🧷***
1. *Distribución Normal:* Genera partículas con posiciones aleatorias basadas en una campana de Gauss.
- Cómo se usó: Se aplicó con randomGaussian() para posicionar partículas cerca del emisor.
- Por qué: Simula patrones naturales más realistas.

2. *Fricción:* Una fuerza que reduce la velocidad de las partículas.
- Cómo se usó: Se calculó un vector opuesto a la dirección del movimiento con un coeficiente de fricción.
- Por qué: Evita que las partículas se muevan infinitamente rápido, haciéndolo más realista.


[Simulación 5](https://editor.p5js.org/Majogc8/sketches/ynrwoGuL6)

![Image](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExcm9xMG9wNW13OWc1c2xyOGg2aXV1cmp2cHNlbXExdG83amVscTN0NSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/muN0n15hcUSw3maPWl/giphy.gif)
