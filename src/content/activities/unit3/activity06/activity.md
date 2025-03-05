#### La fuerza neta debe ser acumulativa

**Enunciado**: ya te diste cuenta entonces que la fuerza neta es la sumatoria de todas las fuerzas que actúan sobre un objeto. Ahora, ¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?

``` js
mover.applyForce(wind);
mover.applyForce(gravity);
.
.
.
``` 

``` js
applyForce(force) {
  // Segunda ley de Newton, pero con acumulación de fuerza, sumando todas las fuerzas de entrada a la aceleración
  this.acceleration.add(force);
}
```

⚠️

Te diste cuenta qué pasó aquí con respecto a la actividad anterior? Vuelve a mirar.

Entonces en cada frame, la aceleración se calcula como la sumatoria de todas las fuerzas que actúan sobre un objeto:

``` js	
mover.applyForce(wind);
mover.applyForce(gravity);
mover.update();
```

Y en el método update() se actualiza la velocidad y la posición del objeto:

``` js
update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
}
```

Pero calma 🧘. Notaste algo raro al final de update()?

- ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
- ¿Por qué se multiplica por cero **justo al final** de update()?

**Entrega**: un texto donde expliques por qué es necesario multiplicar la aceleración por cero al final de cada frame. 
