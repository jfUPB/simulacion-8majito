### Bubble 🫧 

Las burbujas son ligeras, flotan y explotan. Esta palabra inspira una animación donde las letras de la palabra "BUBBLE" se comportan como burbujas flotantes que se inflan y explotan.

**1. Diseña la animación física**

**Representación de la palabra "BUBBLE"**

Usaré círculos para representar las letras de la palabra "BUBBLE". Cada letra será una burbuja que se infla, flota y explota.

**Comportamiento físico**
- Flotación: Las burbujas flotarán en el aire, sin gravedad o con muy poca gravedad, para simular la ligereza de las burbujas.
- Inflado: Cuando el usuario mantenga presionado el clic sobre una letra, esta se inflará gradualmente hasta alcanzar un tamaño máximo.
- Explosión y desaparición: Cuando la burbuja llegue a su tamaño máximo, explotará y desaparecerá de la pantalla.

**Configuración del mundo de Matter.js**
- Gravedad: Se establecerá gravedad baja o nula, para que las burbujas flotan sin caer.
- Límites: Configuraremos los límites del canvas para evitar que las burbujas se salgan del área visible.
- Propiedades de los cuerpos:
   - Masa: Muy baja, para que las burbujas sean ligeras.
   - Elasticidad (restitución): Alta elasticidad, para que las burbujas puedan inflarse sin que se rompan antes de tiempo.
   - Fricción: Muy baja, para que las burbujas se deslicen de manera ligera por el canva

**¿Habrá interacción del usuario?**
- Cuando el usuario mantenga presionado el clic, la letra se irá inflando poco a poco.
- Cuando la letra alcance su tamaño máximo (el límite de inflado), explotará.


[Aplicación](https://editor.p5js.org/Majogc8/sketches/Wbbdzfy0u)

