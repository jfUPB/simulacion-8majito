#### Dos resortes conectados en serie.

- El primer resorte (spring1) sigue conectado a un punto de anclaje fijo
- La primera masa (bob1) conectada al primer resorte
- Un segundo resorte (spring2) que conecta la primera masa con una segunda masa
- La segunda masa (bob2) que cuelga del segundo resorte

Crear nuevos métodos en la clase Spring para permitir la conexión entre dos masas (no solo entre un anclaje fijo y una masa)

##### Metodos:
```js
connectBobs(bob1, bob2) {
  // Vector que apunta desde bob1 hasta bob2
  let force = p5.Vector.sub(bob2.position, bob1.position);
  // Calcular la distancia actual
  let currentLength = force.mag();
  // El estiramiento es la diferencia entre la distancia actual y la longitud de reposo
  let stretch = currentLength - this.restLength;

  // Calcular la magnitud de la fuerza
  let magnitude = -1 * this.k * stretch;
  force.setMag(magnitude);
  
  // Aplicar la fuerza a ambas masas en direcciones opuestas
  bob1.applyForce(force.copy().mult(-1));
  bob2.applyForce(force);
}
```
- Calcula la fuerza del resorte basándose en la distancia entre dos masas (no entre un anclaje y una masa)
- Aplica fuerzas iguales y opuestas a ambas masas (tercera ley de Newton)


- **constrainLengthBetweenBobs(bob1, bob2, minlen, maxlen):**  Mantiene la distancia entre las dos masas dentro de un rango específico, evitando que los resortes se estiren o compriman demasiado.
- **showLineBetweenBobs(bob1, bob2):** Dibuja visualmente el resorte entre las dos masas.


 #### Conexión de los resortes en el programa principal
```js
 // Conectar spring1 a bob1 (como en el código original)
spring1.connect(bob1);

// Conectar bob1 y bob2 mediante spring2 (nuevo)
spring2.connectBobs(bob1, bob2);
```
####  Aplicación de fuerzas y restricciones

- Apliqué la gravedad a ambas masas:
```js
javascriptCopiarlet gravity = createVector(0, 1);
bob1.applyForce(gravity.copy().mult(bob1.mass));
bob2.applyForce(gravity.copy().mult(bob2.mass));
```
- Apliqué restricciones a ambos resortes:
```js
javascriptCopiarspring1.constrainLength(bob1, 20, 300);
spring2.constrainLengthBetweenBobs(bob1, bob2, 20, 300);
```


[Link](https://editor.p5js.org/Majogc8/sketches/w--qmC3_C)

![Image](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExOWdlYmtpdWFsb3E2ZGZwMG5pcjhyanJ5MGw2Nml2c2dudHdwcWU0aSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FWqnyniTHQHTqnOPGj/giphy.gif)
