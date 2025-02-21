Modelar una fuerza en una simulación de sistemas interactivos implica representar matemáticamente cómo actúa una influencia externa sobre los objetos en movimiento. En p5.js y otros entornos de simulación, esto se basa en principios físicos, pero puedes modificar o inventar fuerzas para lograr efectos visuales interesantes.

**Conceptos clave para modelar una fuerza**

1. Fuerza como vector:
- En una simulación, una fuerza se representa como un vector con magnitud y dirección.
- En p5.js, se usa createVector(x, y) para definirla.

2. Segunda Ley de Newton:
- La aceleración de un objeto es directamente proporcional a la fuerza aplicada e inversamente proporcional a su masa:
𝐹=𝑚⋅𝑎⇒𝑎=𝐹𝑚
- Si un objeto tiene más masa, la misma fuerza tendrá menos efecto en su movimiento.

3. Acumulación de fuerzas:
- Las fuerzas no reemplazan la velocidad, sino que se suman como aceleraciones.
- Se aplican en cada frame antes de actualizar la posición.



- Cuando modelamos fuerzas en una simulación interactiva, necesitamos traducir fórmulas físicas en código que pueda aplicarse a los objetos en movimiento. Esto requiere entender no solo la ecuación matemática, sino también cómo convertirla en instrucciones para nuestro sistema.
