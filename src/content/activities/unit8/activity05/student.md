#### Conexión sonido-visión

La conexión fue bastante efectiva porque utilicé las bandas de frecuencia (graves, medios) y el volumen general para controlar parámetros visuales clave como la amplitud y frecuencia de las ondas, y la cantidad de partículas. Esto generó una respuesta visual que se siente sincronizada y “musical”, porque las formas cambian dinámicamente según la música.
Lo más difícil fue que no se sintiera monotono, siempre sentia que le falta algo. Como un movimiento diferente. 

#### Generatividad vs. Control

Para balancear control y generatividad, usé la información del audio como base pero agregué aleatoriedad controlada en la posición y velocidad de partículas, y en la creación de destellos. Así, aunque las visuales respondían al audio, no eran repetitivas ni predecibles. También varié parámetros como la amplitud y frecuencia dentro de rangos, permitiendo cambios suaves y orgánicos.

#### Integración de conceptos

Combiné conceptos de unidades anteriores como:

- Fuerzas y física: para el movimiento natural y rebote de partículas.
- Sistemas y agentes: cada partícula y ola se comporta como un agente autónomo con reglas simples que generan un sistema complejo.
- Simulación interactiva: la visualización responde en tiempo real al input del audio, que es un sistema externo.

#### Desafíos de p5.sound

Un reto fue mantener buen rendimiento porque el análisis FFT y la generación de muchas partículas consumen recursos y se me buggeaba el programa. También la precisión del análisis depende de la calidad del audio. La API es sencilla pero requiere práctica para interpretar bien las bandas y niveles para que la visualización sea fluida.

#### Resultado final

El resultado se acerca mucho a la idea visual original de un flujo orgánico y musical que reacciona al piano. Me gusto cómo las ondas y partículas se sienten vivas y sincronizadas con el audio.
Si tuviera más tiempo, mejoraría la optimización para tener más partículas y agregaría más variedad visual para que la experiencia sea aún más rica y menos repetitiva.
