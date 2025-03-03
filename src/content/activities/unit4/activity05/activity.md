#### Coordenadas polares

**Enunciado**: explora otro sistema de coordenadas útil cuando se trabaja con ángulos. Se trata de las coordenadas polares.

Considera esta simulación de [coordenadas polares](https://editor.p5js.org/juanferfranco/sketches/fE5rCtDS1):

- Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes 
repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.

``` js
function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  // Convert polar to cartesian
  let x = r * cos(theta);
  let y = r * sin(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(x, y, 48);
  theta += 0.02;
}
```
 
 Modifica la función ``draw()``:

``` js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, x, y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

¿Qué ocurre? ¿Por qué?

Ahora realiza esta modificación:

``` js
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta,r);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, v.x, v.y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

- ¿Qué ocurre aquí? ¿Por qué?

**Entrega**: la respuesta a las preguntas anteriores.
