#### Matter.js 
Probar fisica realista para representar colisiones, friccion, gravedad entre objetos rigidos(rectangulos, circulos,cuadrados,poligonos)
Seven keys components 
#### Conceptos clave: 
- *Engine:* El corazon del sistema,tiene métodos para crear y manipular motores. Un motor es un controlador que gestiona la actualización de la simulación del mundo. Es el cerebro de todo el proceso que calcula cómo se mueven los objetos, las colisiones, la gravedad, etc. Sin el Engine, no tendrías ninguna física en la simulación.
- *World:* Es el espacio donde se colocan todos los cuerpos y restricciones. Se crea automáticamente al inicializar el Engine. Todo lo que se ponga en el World va a interactuar y seguir las reglas de física que el Engine determina.
- *Body:* Objeto individual dentro de la simulación. Es como una letra o un objeto físico que se comporta según las leyes de la física. Puede ser una figura sencilla como un círculo, un rectángulo o una forma más compleja. Cada Body tiene propiedades como masa, tamaño y forma.
- *Bodies (y sus tipos: rectángulos, círculos, polígonos):* métodos de fábrica para crear modelos de cuerpo rígido con configuraciones de cuerpo de uso común (como rectángulos, círculos y otros polígonos).Matter.Bodies. No es un solo cuerpo, sino un conjunto de funciones para crear diferentes tipos de cuerpos físicos. 

- *Constraint:* Métodos para crear y manipular restricciones. Las restricciones se utilizan para especificar que se debe mantener una distancia fija entre dos cuerpos (o entre un cuerpo y una posición fija en el espacio universal). La rigidez de las restricciones se puede modificar para crear muelles o elásticos.Matter.Constraint.
- *MouseConstraint:*  Métodos para crear restricciones del ratón. Las restricciones del mouse se utilizan para permitir la interacción del usuario, proporcionando la capacidad de mover cuerpos a través del mouse o el tacto.Matter.MouseConstraint
- *Runner/Events:* Render es el componente visual que dibuja la simulación en la pantalla.Matter.Events. El Runner asegura que el motor de física se actualice de manera constante, lo que permite que las animaciones y simulaciones sigan funcionando sin interrupciones. Esto hace que la experiencia de usuario sea más fluida, porque el motor se sincroniza con la velocidad de fotogramas (FPS) del navegador.

 p5.js 
 ```js
 <script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.20.0/matter.min.js">
 ```

 #### Experimentación con código 🐊

 **1.mundo con gravedad y añadir cuerpos simples (poligonos) que caigan y colisionen.**

[Ejemplo1](https://editor.p5js.org/Majogc8/sketches/jc3lGogHh)
