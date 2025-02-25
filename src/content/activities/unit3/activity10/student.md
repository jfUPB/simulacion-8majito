#### Fricción 🤸
En la simulación de los globos con fricción y viento, hemos modelado diferentes fuerzas utilizando vectores en p5.js

Cuando mezclamos todas estas fuerzas: 
- Gravedad hace que los globos caigan.
- Viento empuja los globos de forma dinámica.
- Fricción ralentiza los globos cuando tocan el suelo.
- Rebote impide que los globos salgan del canvas.

1. Gravedad: La gravedad es una fuerza que siempre apunta hacia abajo, empujando los objetos hacia la parte inferior del canvas.
```js
let gravedad = createVector(0, 0.2);
globo.applyForce(gravedad);
```
Explicación: Se crea un vector de gravedad con dirección hacia abajo (0,0.2) y se aplica a cada globo.

2.Fuerza viento: El viento es una fuerza horizontal que empuja los globos hacia la derecha cuando hacemos click.
```js
function mousePressed() {
  let viento = createVector(random(3, 5), random(-1, 1)); 
  for (let globo of globos) {
    globo.applyForce(viento);
  }
}
```
Explicación: Al presionar el mouse, se genera un vector de viento con una dirección aleatoria hacia la derecha y un ligero movimiento vertical. Esto hace que el comportamiento del viento sea más dinámico.

3. Fuerza de Fricción: La fricción es una fuerza que se opone al movimiento cuando los globos tocan el suelo.
```js
let c = 0.1;
let friction = this.velocity.copy().mult(-1);
friction.setMag(c);
this.applyForce(friction);
```
Explicación: Se toma la velocidad actual del globo, se invierte su dirección (para que se oponga al movimiento) y se ajusta su magnitud usando el coeficiente de fricción. Luego, la fuerza se aplica al objeto.
 
[Ejemplo 1](https://editor.p5js.org/Majogc8/sketches/LIOVEYmlW)
