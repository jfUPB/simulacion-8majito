#### 1. ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?
- Cuando inicializo el Emitter, se crean todas las partículas basándome en los colores y la estructura de la imagen. Las partículas se guardan en un arreglo para poder manejarlas fácilmente.
- Por otro lado, las partículas desaparecen cuando su propiedad lifespan llega a cero. Uso splice() para eliminarlas del arreglo, así no ocupan memoria innecesariamente. Esto mantiene el programa eficiente porque no dejo que se acumulen partículas que ya no están activas.

#### 2. ¿Cómo se aplica el marco de movimiento Motion 101 en los sistemas de partículas que analizaste en la actividad anterior?
- Motion 101 siempre esta aplicado,  usé el concepto de aceleración y fuerza acumulativa.
- Cada partícula tiene una posición, una velocidad, y una aceleración. La aceleración se acumula cuando aplico fuerzas, como cuando las partículas se mueven hacia su objetivo (su posición original en la imagen) o cuando les aplico la fuerza del "efecto agua" alrededor del mouse.
- El movimiento es suave y fluido porque uso vectores para calcular la dirección y magnitud de las fuerzas, además de amortiguar la velocidad con un multiplicador para simular resistencia.
- Este proceso le da vida a las partículas y hace que se muevan de una manera suave, como si estuvieran siguiendo un camino lógico en lugar de ir por ahí de forma caótica.

#### 3. ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?

Añadí dos tipos de fuerzas externas: 
- Atracción hacia un objetivo(La imagen): Cada partícula siente una fuerza hacia su posición original en la imagen. 
- Queria experimentar como cuando uno toca el agua y es ese movimiento del agua que se llama Repulsión: Cuando muevo el mouse, las partículas cercanas sienten una fuerza de repulsión y calcula la magnitud de la fuerza. Si la distancia entre la partícula y el mouse es pequeña, el valor de la división será grande(Se movera del todo). Si la distancia es grande, el valor de la división será pequeño (fNos e mueve ya que esta lejos).
- Ambas fuerzas están modeladas con vectores, y Motion 101 se encarga de que todo fluya de manera natural.

#### 4. ¿Cómo aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?
- No usé herencia al principio por que no sabia que hacer, pero luego decidí crear la clase Confetti(Usada de la unidad anterior) que hereda de Particle. Esto me permitió reutilizar todo el comportamiento básico de las partículas, como el movimiento y la vida útil, y personalizar solo lo que necesitaba, que fue el cambio de color.
- Gracias a la herencia, puedo tratar a todas las partículas de la misma forma. Cuando recorro el arreglo, no importa si son del tipo Particle o Confetti, ya que ambas tienen los mismos métodos (update y show). Así, las partículas normales y el confetti funcionan juntas sin complicaciones.

