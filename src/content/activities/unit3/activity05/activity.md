#### ¿Y qué relación tiene esto de las leyes de Newton con al arte generativo?

**Enunciado**: analiza con detenimiento la siguiente idea.

Antes de comenzar, disfruta esta [simulación](https://wipaweeeeee.github.io/creativeCoding/lawsOfAttraction/index.html) donde puedes experimentar de manera creativa las leyes de la atracción. Ahora si, volviendo 
al lío de Monte Pío (pero no tanto). 

La ecuación vectorial de la segunda ley de Newton se expresa como:

$$
\sum \vec{F} = m \vec{a}
$$

Donde:

- La fuerza neta o la sumatoria de todas las fuerzas que actúan sobre un objeto:

  $$
  \sum \vec{F}
  $$

- Masa del objeto:

  $$
  m
  $$

- Aceleración del objeto:

  $$
  \vec{a}
  $$

Nota entonces que: 

$$
\vec{a} = \frac{\sum \vec{F}}{m}
$$

Y lo siguiente lo dejaré a tu criterio, finalmente tu eres quien toma las decisiones en tu 
mundo de arte generativo ¿Qué tal si la masa es igual a 1? Yo no veo por qué no, finalmente, 
en el mundo de los pixeles el artista manda (¿O no?) 😉.

$$
\vec{a} = \sum \vec{F}
$$

Entonces ya tenemos la relación. En la unidad anterior tu definías en cada **frame** 
de la simulación un algoritmo para la aceleración. Ahora, la aceleración en cada frame la 
calcularemos como la influencia de todas las fuerzas sobre nuestros elementos gráficos 😹.

Si volvemos a nuestro texto guía: The Nature of code, verás que un elemento gráfica que se mueva 
en el canvas tendrá mínimo estas propiedades:

``` js
class Mover {
  constructor() {
    this.position = createVector();
    this.velocity = createVector();
    this.acceleration = createVector();
  }
}
```

Ahora, considera que en un frame actúan sobre este elemento dos fuerzas: viento y gravedad. Por 
tanto, en ese frame aplicarás las dos fuerzas:

``` js
mover.applyForce(wind);
```

y luego:

``` js
mover.applyForce(gravity);
```	

Finalmente, en el método `applyForce` de la clase `Mover` tendrás algo como:

``` js
applyForce(force) {
  this.acceleration = force;
}
```

Y listo ¿Cierto?

**Entrega**: ¿Qué problema le ves a este planteamiento? ¿Qué solución propones? ¿Cómo lo implementarías en p5.js?

Nota: no olvides que queremos calcular la aceleración en cada frame como la sumatoria de todas las fuerzas que actúan sobre un objeto.




