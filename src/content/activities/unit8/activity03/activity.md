#### Diseño del algoritmo generativo (proceso y outputs)

:::note[🎯 Enunciado]
Continuamos el diseño definiendo el corazón de tu sistema: el algoritmo generativo que transformará los inputs de audio y de interacción en los visuales dinámicos. Aplicaremos aquí todo lo aprendido en el curso sobre simulación y generación.
:::

:::tip[Recursos]
-   Tu concepto e inputs definidos en la Actividad 02.
-   Tu conocimiento de TNoC y unidades anteriores: fuerzas, partículas, sistemas, agentes autónomos (flow fields, flocking), física (Matter.js si quieres), ruido Perlin, aleatoriedad controlada, etc.
:::

👣 **Pasos**:

1.  **Diseña el proceso generativo:** ¿Cuál será la lógica central que genere los visuales?
    *   Elige uno o varios **conceptos/técnicas de simulación o generación** del curso que se alineen con tu concepto visual (ej: sistema de partículas cuyas fuerzas son moduladas por el FFT, un flow field cuya dirección cambia con la amplitud, agentes tipo boids cuya cohesión depende de los graves, formas que crecen usando ruido Perlin modificado por el ritmo...).
    *   Describe cómo los **inputs de audio** y de **interacción** **modularán los parámetros** clave de tu algoritmo generativo. Sé específico (ej: "La amplitud controlará el número de partículas emitidas", "La energía en los agudos del FFT aumentará la velocidad máxima de los agentes", "El mouseX cambiará el factor de ruido en el flow field").
    *   Asegúrate de que tu diseño incluya elementos **generativos** que hagan que la visualización no sea idéntica cada vez, incluso con la misma música (ej: uso de aleatoriedad controlada, ruido, condiciones iniciales variables).
2.  **Define los outputs visuales:**
    *   ¿Qué elementos visuales específicos se generarán? (partículas, líneas, formas, colores, texturas...).
    *   ¿Qué **propiedades** de estos elementos serán **controladas dinámicamente** por el proceso generativo (y, por ende, por los inputs)? (posición, tamaño, color, opacidad, velocidad, conexión, etc.).
3.  **Documenta el diseño:** describe tu algoritmo (proceso) y los visuales resultantes (outputs) de forma clara. Puedes usar:
    *   **Texto detallado.**
    *   **Pseudocódigo** para la lógica clave.
    *   **Diagramas de flujo o conceptuales** que muestren cómo los inputs afectan al proceso y éste a los outputs.

:::note[🧐🧪✍️ Reporta en tu bitácora]

-   La descripción detallada de tu **algoritmo generativo (proceso)**: qué técnicas usarás y, fundamentalmente, **cómo los inputs de audio e interacción modularán sus parámetros**. Explica cómo asegurarás la naturaleza generativa (no repetitiva).
-   La descripción de los **outputs visuales**: qué elementos se generan y qué propiedades cambiarán dinámicamente.
-   Incluye pseudocódigo o diagramas si te ayudan a clarificar tu diseño.
:::

:::caution[📤 Entrega]
El diseño detallado de tu algoritmo generativo (proceso) y los outputs visuales, explicando la conexión con los inputs y la naturaleza generativa, documentado en tu bitácora.
:::
