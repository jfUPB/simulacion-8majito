#### Fricción 🤸
En la simulación de los globos con fricción y viento, hemos modelado diferentes fuerzas utilizando vectores en p5.js

Cuando mezclamos todas estas fuerzas: 
- Gravedad hace que los globos caigan.
- Viento empuja los globos de forma dinámica.
- Fricción ralentiza los globos cuando tocan el suelo.
- Rebote impide que los globos salgan del canvas.

**1. Gravedad:**  La gravedad es una fuerza que siempre apunta hacia abajo, empujando los objetos hacia la parte inferior del canvas.
```js
let gravedad = createVector(0, 0.2);
globo.applyForce(gravedad);
```
Explicación: Se crea un vector de gravedad con dirección hacia abajo (0,0.2) y se aplica a cada globo.

**2.Fuerza viento:**  El viento es una fuerza horizontal que empuja los globos hacia la derecha cuando hacemos click.
```js
function mousePressed() {
  let viento = createVector(random(3, 5), random(-1, 1)); 
  for (let globo of globos) {
    globo.applyForce(viento);
  }
}
```
Explicación: Al presionar el mouse, se genera un vector de viento con una dirección aleatoria hacia la derecha y un ligero movimiento vertical. Esto hace que el comportamiento del viento sea más dinámico.

**3. Fuerza de Fricción:**  La fricción es una fuerza que se opone al movimiento cuando los globos tocan el suelo.
```js
let c = 0.1;
let friction = this.velocity.copy().mult(-1);
friction.setMag(c);
this.applyForce(friction);
```
Explicación: Se toma la velocidad actual del globo, se invierte su dirección (para que se oponga al movimiento) y se ajusta su magnitud usando el coeficiente de fricción. Luego, la fuerza se aplica al objeto.
 
[Ejemplo 1](https://editor.p5js.org/Majogc8/sketches/LIOVEYmlW)

#### Resistencia del aire y de fluidos
**1. Gravedad:** La gravedad es una fuerza constante que tira del barco hacia abajo. En la simulación, esta fuerza se modela como un vector que apunta hacia abajo con una magnitud de 0.3.
   
```js
let gravedad = createVector(0, 0.3);
barco.applyForce(gravedad);
```

**2. Resistencia aire:** Antes de que el barco toque el agua, el aire ejerce una resistencia que se opone a su movimiento. La resistencia se basa en la velocidad del barco, aplicando una fuerza en la dirección opuesta.

```js
if (barco.position.y < aguaNivel) {
  let aire = barco.velocity.copy().mult(-0.03);
  barco.applyForce(aire);
}
```

Esta resistencia evita que el barco caiga demasiado rápido y simula el efecto del aire al moverse.

**3. Resistencia del Agua:** Cuando el barco toca el agua, la resistencia es mayor debido a la densidad del líquido. Se modela de manera similar a la resistencia del aire, pero con un coeficiente mayor para representar el arrastre del agua.
```
if (barco.position.y >= aguaNivel) {
  let agua = barco.velocity.copy().mult(-0.2);
  barco.applyForce(agua);
}
```
**4. Flotabilidad:** Si el barco se hunde demasiado, el agua ejerce una fuerza de flotabilidad hacia arriba. La magnitud de esta fuerza es proporcional a la profundidad a la que se encuentra el barco bajo el nivel del agua.

```js
let profundidad = barco.position.y - aguaNivel;
let flotabilidad = createVector(0, -0.25 * profundidad);
barco.applyForce(flotabilidad);
```
Esto impide que el barco se hunda completamente y genera un efecto de oscilación sobre las olas. Ademas, esta fuerza ralentiza el movimiento del barco en el agua.

**5. Viento (cuando el usuario mantiene presionado el clic):** El viento se modela como una fuerza horizontal que empuja el barco hacia la derecha. En lugar de aplicarse solo cuando el usuario hace clic, se mantiene mientras el mouse esté presionado.

```js
if (mouseIsPressed) {
  let viento = createVector(0.5, 0);
  barco.applyForce(viento);
```
[Ejemplo 2](https://editor.p5js.org/Majogc8/sketches/whpCPCgrZ)

#### Atracción gravitacional:

En esta simulación, se modelo un sistema de partículas (movers) que son atraídas por un centro de gravedad (attractor), simulando la dinámica de un tornado con un efecto de rotación adicional.

**1. Fuerza Gravitacional:** La atracción entre el tornado y las partículas se basa en la Ley de la Gravitación Universal, donde la fuerza de atracción entre dos objetos depende de sus masas e inversamente del cuadrado de la distancia entre ellos. Se calcula con la fórmula:

𝐹=𝑀𝑚/𝑑^2

- M es la masa del tornado (attractor)
- m es la masa de la partícula (mover)
- d es la distancia entre ellos (limitada para evitar fuerzas extremadamente altas o débiles).

**2. Efecto de Rotación:** Para imitar la naturaleza giratoria de un tornado, modificamos la dirección de la fuerza gravitacional con un pequeño desplazamiento angular. Esto se hace al sumar un ángulo al vector de fuerza original, generando un efecto de curva en el movimiento de las partículas:

```js
let angle = QUARTER_PI / 2; 
let rotatedForce = p5.Vector.fromAngle(force.heading() + angle);
rotatedForce.setMag(force.mag());
return rotatedForce;
```
**3. Actualización de Movimiento:** Cada partícula (mover) responde a la fuerza aplicada según la Segunda Ley de Newton:

𝐴=𝐹/𝑚

```js
​let f = p5.Vector.div(force, this.mass);
this.acceleration.add(f);
 ```
Después, se actualiza su velocidad y posición en cada fotograma:

```js
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
this.acceleration.mult(0);
```
- ​El Attractor se representa como un círculo azul en el centro.
- Los Movers son pequeños círculos rojos que orbitan y convergen en espiral.
 

[Ejemplo 3](https://editor.p5js.org/Majogc8/sketches/Oagj9ZWsP)
