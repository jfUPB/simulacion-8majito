#### Implementación

:::note[🎯 Enunciado]
Es el momento de implementar tu diseño y crear la simulación musical generativa en tiempo real 
usando p5.js y p5.sound, aplicando los conceptos y técnicas que has diseñado.
:::

:::tip[Recursos]
-   Tu diseño completo de las Actividades 02 y 03 (Concepto, Inputs, Proceso, Outputs).
-   Tu entorno p5.js.
-   Librería p5.sound (para `Amplitude`, `FFT`, cargar/reproducir sonido o usar micrófono). Revisa su [referencia](https://p5js.org/reference/p5.sound/).
-   Tu código y conocimientos de unidades anteriores (partículas, agentes, física, etc.).
:::

👣 **Pasos**:

1.  **Configura p5.sound:**
    *   Carga el archivo de música elegido.
    *   Inicializa los objetos de análisis que necesitas (`p5.Amplitude`, `p5.FFT`).
2.  **Implementa el algoritmo generativo (proceso):**
    *   Traduce la lógica de tu diseño (Actividad 03) a código p5.js. Crea las clases o funciones necesarias para tus partículas, agentes, sistemas, etc.
    *   En `draw()`, obtén los valores actuales de los inputs de audio.
    *   Usa esos valores de audio para **modular en tiempo real** los parámetros de tu algoritmo generativo, tal como lo diseñaste.
3.  **Genera los outputs visuales:**
    *   Implementa el código de dibujo que genera los elementos visuales (partículas, líneas, formas...).
    *   Asegúrate de que las propiedades visuales (color, tamaño, posición, etc.) sean controladas por el estado de tu algoritmo generativo.
4.  **Implementa la interacción:** añade los manejadores de eventos (`mousePressed`, `mouseMoved`, `keyPressed`, etc.) para los inputs de interacción que diseñaste.
5.  **Prueba, depura y refina:** ejecuta tu sketch con la música.
    *   Verifica que el análisis de audio funciona y que los visuales responden a los cambios en la música como esperabas.
    *   Ajusta la sensibilidad de la respuesta al audio, los parámetros del algoritmo y la estética visual hasta que estés satisfecho con el resultado. Asegúrate de que se perciba la naturaleza generativa (no exactamente igual cada vez).

:::note[🧐🧪✍️ Reporta en tu bitácora]

-   Explica brevemente cómo **configuraste p5.sound** y qué datos de audio estás extrayendo.
-   Muestra **fragmentos de código clave** donde los datos del audio modulan tu algoritmo generativo y donde el algoritmo controla los outputs visuales.
-   Incluye el **código completo** de tu sketch final (o enlace a repositorio).
-   Inserta un **VIDEO DEMO** (si esta vez si, pero trata de optimizar el archivo) (¡esencial!) de tu simulación en acción, **CON la música sonando**. Debe mostrar la respuesta al audio y la naturaleza generativa.
:::

:::caution[📤 Entrega]
Tu simulación implementada (explicación de puntos clave, código completo y video con audio), documentado en tu bitácora.
:::
