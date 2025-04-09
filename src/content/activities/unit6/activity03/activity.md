#### Analizando el comportamiento de enjambre (Flocking)

:::note[🎯 Enunciado]
Ahora analizaremos el segundo algoritmo: el comportamiento de enjambre (Flocking), 
famoso por simular el movimiento coordinado de pájaros o peces. Nos basaremos nuevamente en 
"The Nature of Code" para entender las tres reglas básicas que lo gobiernan.
:::

:::tip[Recursos]
-   Libro "The Nature of Code" (TNoC) de Daniel Shiffman: [capítulo 5, sección "Flocking"](https://natureofcode.com/autonomous-agents/#flocking) (y ejemplos de código asociados).
-   El código fuente del ejemplo de Flocking de TNoC.
-   Tu entorno p5.js.
:::

👣 **Pasos**:

1.  **Ejecuta el ejemplo:** ejecuta el código del ejemplo principal de Flocking de TNoC en p5.js. Observa el movimiento colectivo de los "boids" (agentes).
2.  **Identifica las tres reglas:** en el código de la clase del agente (ej: `Boid`), localiza las funciones que implementan las tres reglas fundamentales del Flocking:
    *   **Separación (Separation):** evitar el hacinamiento con vecinos cercanos.
    *   **Alineación (Alignment):** dirigirse en la misma dirección promedio que los vecinos cercanos.
    *   **Cohesión (Cohesion):** moverse hacia la posición promedio de los vecinos cercanos.
3.  **Explica las Reglas:** para cada una de las tres reglas, explica con tus propias palabras:
    *   ¿Cuál es el objetivo de la regla?
    *   ¿Cómo calcula el agente la fuerza de dirección correspondiente? (describe la lógica general, ej: "Calcula un vector apuntando lejos de los vecinos demasiado cercanos").
4.  **Identifica parámetros clave:** localiza en el código las variables que controlan:
    *   El radio (o distancia) de percepción (`perceptionRadius` o similar) que define quiénes son los "vecinos". A veces también hay un ángulo de percepción.
    *   Los pesos o multiplicadores que determinan la influencia relativa de cada una de las tres reglas al combinarlas.
    *   La velocidad máxima (`maxspeed`) y la fuerza máxima (`maxforce`) de los agentes (similar a Flow Fields).
5.  **Experimenta con modificaciones:** realiza al menos **una** de las siguientes modificaciones en el código, ejecuta y describe el efecto observado en el comportamiento *colectivo* del enjambre:
    *   Cambia drásticamente el peso de una de las reglas (ej: pon la cohesión a cero, o la separación muy alta).
    *   Modifica significativamente el radio de percepción (hazlo muy pequeño o muy grande).
    *   Introduce un objetivo (`target`) que todos los boids intenten seguir (usando una fuerza de `seek`) además de las reglas de flocking, y ajusta su influencia.

:::note[🧐🧪✍️ Reporta en tu bitácora]

-   Explica con tus palabras el objetivo y la lógica general de cálculo de cada una de las tres reglas de Flocking (Separación, Alineación, Cohesión).
-   Lista los parámetros clave identificados (radio de percepción, pesos de las reglas, maxspeed, maxforce).
-   Describe la modificación que realizaste al código y **explica detalladamente el efecto** que tuvo en el comportamiento *colectivo* del enjambre (¿se dispersan? ¿forman grupos compactos? ¿se mueven caóticamente?). Incluye una captura de pantalla o GIF si ilustra bien el cambio. Muestra el fragmento de código modificado.
:::

:::caution[📤 Entrega]
Tu análisis del algoritmo de Flocking, incluyendo la explicación de sus reglas, la identificación de parámetros y la descripción de tu experimento de modificación, todo documentado en tu bitácora.
:::