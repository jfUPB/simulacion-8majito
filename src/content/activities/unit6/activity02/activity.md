#### Analizando los campos de flujo (flow fields)

:::note[🎯 Enunciado]
Vamos a analizar el primer algoritmo clave: los campos de flujo (Flow Fields), basándonos en el ejemplo del libro "The Nature of Code". Entenderemos cómo una cuadrícula de vectores dirige el movimiento de los agentes.
:::

:::tip[Recursos]
-   Libro "The Nature of Code" (TNoC) de Daniel Shiffman: [Capítulo 5, sección "Flow Fields"](https://natureofcode.com/autonomous-agents/#flow-fields) (y ejemplos de código asociados).
-   El código fuente del ejemplo de Flow Fields de TNoC.
-   Tu entorno p5.js.
:::

👣 **Pasos**:

1.  **Ejecuta el ejemplo:** ejecuta el código del ejemplo principal de Flow Fields de TNoC en p5.js. Observa el comportamiento de los vehículos/agentes.
2.  **Identifica la estructura del campo:** en el código (usualmente en una clase `FlowField`), localiza cómo se almacena el campo de flujo. ¿Qué estructura de datos se usa (ej: un array 2D)? ¿Qué representa cada elemento de esa estructura? ¿Cómo se calcula inicialmente el vector en cada punto?
3.  **Analiza el comportamiento del agente:** en el código de la clase del vehículo/agente (`Vehicle`), encuentra la función `follow()`. Explica con tus palabras:
    *   ¿Cómo determina el agente qué vector del campo de flujo debe seguir basándose en su posición actual? (pista: implica mapear la posición a índices de la cuadrícula).
    *   Una vez que tiene el vector deseado del campo, ¿cómo lo utiliza para calcular la fuerza de dirección (`steering force`)? (pista: implica calcular la diferencia con la velocidad actual y limitar la fuerza).
4.  **Identifica parámetros clave:** Localiza en el código las variables que controlan aspectos importantes como:
    *   La resolución del campo de flujo (el tamaño de las celdas de la cuadrícula).
    *   La velocidad máxima (`maxspeed`) y la fuerza máxima (`maxforce`) de los agentes.
5.  **Experimenta con modificaciones:** realiza al menos **una** de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento de los agentes:
    *   Cambia significativamente la forma en que se generan los vectores del campo (ej: usa una fórmula matemática diferente en lugar de `noise()`, o cambia drásticamente los parámetros de `noise()`).
    *   Modifica sustancialmente la resolución del campo de flujo (hazla mucho más fina o mucho más gruesa).
    *   Altera considerablemente `maxspeed` o `maxforce` de los agentes.

:::note[🧐🧪✍️ Reporta en tu bitácora]

-   Explica brevemente la estructura de datos usada para el campo de flujo y cómo se generan sus vectores.
-   Describe con tus palabras cómo un agente utiliza el campo para calcular su fuerza de dirección.
-   Lista los parámetros clave identificados (resolución, maxspeed, maxforce).
-   Describe la modificación que realizaste al código y **explica detalladamente el efecto** que tuvo en el movimiento y comportamiento colectivo de los agentes. Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
:::

:::caution[📤 Entrega]
Tu análisis del algoritmo de Flow Fields, incluyendo la explicación de su funcionamiento, la identificación de parámetros y la descripción de tu experimento de modificación, todo documentado en tu bitácora.
:::