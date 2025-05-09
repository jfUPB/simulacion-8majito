#### Animando la tipografía semántica

:::note[🎯 Enunciado]
¡Es hora de aplicar todo! Elige una palabra y, usando p5.js y Matter.js, 
crea una animación donde la palabra "actúe" o se comporte físicamente de una manera 
que refleje su significado, inspirándote en el concepto "Word as Image".
:::

:::tip[Recursos]
-   Tus ideas estáticas de la Actividad 01.
-   Tu conocimiento y código de experimentación de Matter.js (Actividad 02).
-   Documentación de Matter.js.
-   Tu creatividad y habilidades de p5.js.
:::

👣 **Pasos**:

1.  **Elige tu palabra:** selecciona una palabra cuyo significado te inspire una idea 
de animación física (ej: "caer", "flotar", "romper", "elástico", "rígido", "conectar", "repeler").
2.  **Diseña la animación física:**
    *   ¿Cómo representarás la palabra? ¿Usarás cuerpos de Matter.js para formar las letras?
    *   ¿Qué **comportamiento físico** (caída, flotación, colisión, elasticidad, ruptura, conexión) representará el significado?
    *   ¿Cómo configurarás el mundo de Matter.js (gravedad, límites) y las propiedades de los cuerpos (masa, fricción, restitución/elasticidad) para lograr ese comportamiento?
    *   ¿Habrá alguna **interacción** del usuario (ej: click para iniciar la acción, mouse para perturbar)?
3.  **Implementa en p5.js + Matter.js:** escribe el código.
    *   Configura el motor y mundo de Matter.js.
    *   Crea los cuerpos (`Bodies`) que forman tu palabra. Esto puede requerir paciencia y experimentación para obtener las formas deseadas. Usa `Constraint` si necesitas unir partes.
    *   Define las propiedades físicas y las condiciones iniciales.
    *   Implementa la interacción si la diseñaste.
    *   Dibuja los cuerpos de Matter.js en el canvas de p5.js en cada frame.
4.  **Prueba y Refina:** ejecuta tu sketch repetidamente. Ajusta la gravedad, las propiedades de los cuerpos, las restricciones y cualquier otro parámetro hasta que la animación física comunique efectivamente el significado de la palabra de una manera interesante.

:::note[🧐🧪✍️ Reporta en tu bitácora]

-   Indica claramente la **palabra elegida**.
-   Explica tu **idea conceptual**: ¿Cómo la animación física representa el significado de la palabra?
-   Describe brevemente los **aspectos técnicos clave** de tu implementación: ¿Cómo formaste las letras con Matter.js? ¿Qué propiedades físicas fueron importantes? ¿Usaste restricciones?
-   Incluye el **código completo** de tu sketch final.
-   Inserta una **captura de pantalla estática** Y un **enlace a un GIF animado** (¡Esencial!) que muestre tu tipografía semántica animada en acción.
:::

:::caution[📤 Entrega]
Tu proyecto final de tipografía semántica animada (concepto, explicación técnica, código y demo GIF), documentado en tu bitácora.
:::
