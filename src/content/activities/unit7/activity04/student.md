**Flujo de trabajo**
Para integrar Matter.js con p5.js, primero configuré el motor de física en setup() usando Engine.create(). También creé los muros del canvas como límites físicos y las letras como cuerpos circulares. En draw(), actualizo el motor de Matter.js con Matter.Runner.run(engine) (una sola vez) y luego, en cada frame, uso la posición y el ángulo de cada cuerpo para dibujar las letras con p5.js.

**Representación visual vs. simulación física**
Cada burbuja (letra) tiene un cuerpo físico de Matter.js y una visualización personalizada con ellipse() y text() en p5.js. Usé body.position y body.angle para alinear correctamente las letras en el canvas. Un reto fue escalar los cuerpos cuando crecen (al mantener clic) y sincronizarlo visualmente sin que se deformaran o rompieran la simulación.

**Creación de formas complejas**
Para representar las letras usé círculos simples, y sobre ellos dibujé el carácter con text(). 

**Física para la semántica**
Usar física fue muy efectivo para representar la palabra "BUBBLE" porque las burbujas se mueven, flotan y explotan como se espera de una burbuja real. Este tipo de enfoque funciona bien con conceptos relacionados con movimiento, ligereza, caos o interacción (como "SALTO", "CAER", "GLOBO"). Sería más difícil representar palabras abstractas o estáticas como “CALMA” o “SILENCIO”.

**Potencial exploratorio**
La combinación de p5.js y Matter.js permite crear experiencias interactivas, educativas y artísticas. Se podría explorar tipografía reactiva al sonido, juegos con palabras que se destruyen o se arman, instalaciones digitales donde el público mueve letras con el cuerpo (usando cámaras), o visualizaciones de emociones con física.

