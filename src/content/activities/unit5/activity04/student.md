#### 1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?
- Cuando inicializo el Emitter, se crean todas las partículas basándome en los colores y la estructura de la imagen. Las partículas se guardan en un arreglo para poder manejarlas fácilmente.
- Por otro lado, las partículas desaparecen cuando su propiedad lifespan llega a cero. Uso splice() para eliminarlas del arreglo, así no ocupan memoria innecesariamente. Esto mantiene el programa eficiente porque no dejo que se acumulen partículas que ya no están activas.

#### 2. ¿Cómo se aplica el marco de movimiento Motion 101 en los sistemas de partículas que analizaste en la actividad anterior?
- Lo que hago es que cada partícula calcula una fuerza hacia su posición objetivo usando vectores. Esa fuerza se acumula en la aceleración, y luego actualizo la velocidad y la posición usando esa aceleración (tal como dice Motion 101). Además, le agrego algo de "fricción" al multiplicar la velocidad por 0.95, para que el movimiento se sienta más natural y fluido.
- Este proceso le da vida a las partículas y hace que se muevan de una manera suave, como si estuvieran siguiendo un camino lógico en lugar de ir por ahí de forma caótica.

