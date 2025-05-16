#### ¿Por qué multiplicamos la aceleración por cero al final de update()?

Es necesario multiplicar la aceleración por cero porque, a diferencia de la velocidad y la posición, la aceleración no tiene memoria. La aceleración en cada frame depende únicamente de las fuerzas que están actuando en ese momento. Si no la reiniciamos, las fuerzas de los frames anteriores seguirían acumulándose, lo que haría que el objeto se acelerara de manera irreal.

En otras palabras, cada frame debe calcular la aceleración desde cero, sumando solo las fuerzas actuales. Esto garantiza que el comportamiento del objeto siga las leyes del movimiento de Newton de forma correcta.

#### ¿Por qué se multiplica por cero justo al final de update()?

Se multiplica por cero justo al final de update() porque en este método primero se actualiza la velocidad y la posición del objeto usando la aceleración acumulada en el frame actual. Una vez que se han aplicado todas las fuerzas y el objeto se ha movido, la aceleración debe reiniciarse para el siguiente frame.

Si lo hiciéramos antes, la aceleración ya no estaría disponible para actualizar la velocidad y la posición en ese frame. Por eso, la operación this.acceleration.mult(0); se coloca al final de update(), asegurando que las fuerzas acumuladas solo afecten el movimiento del objeto en ese instante y no en los frames siguientes.

Esto permite que cada frame sea independiente y que la simulación refleje correctamente la influencia de las fuerzas en el momento en que actúan.
