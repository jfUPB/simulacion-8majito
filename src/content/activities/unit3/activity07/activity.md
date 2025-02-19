#### En mi mundo los pixeles si tienen masa

**Enunciado**:

Como en tu mundo los pixeles tienen masa, entonces, ¿Qué pasa si en un frame actúan sobre un objeto dos fuerzas? ¿Cómo calculas la aceleración resultante?

``` js
mover.applyForce(wind);
mover.applyForce(gravity);
```

``` js
applyForce(force) {
    // Asume que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```

¿Y listo cierto? Pues ¡No! 🤣 

¿Qué ves raro? 

**Entrega**: un texto donde expliques qué problema le ves a este planteamiento y qué solución propones. ¿Cómo lo implementarías en p5.js?

Nota: recuerda, ¿Cuándo se pasa algo a un función por valor y cuándo por referencia? En este caso, **force** es 
objeto de la clase p5.Vector, es decir, es un objeto que se pasa por referencia. ¿Qué implica esto?

