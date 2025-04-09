#### Flocking
- Fórmula de la fuerza de dirección (dirección = deseada - velocidad) para implementar las reglas de flocado.
- Estas fuerzas de dirección serán comportamientos grupales y requerirán que cada vehículo perciba a todos los demás vehículos.
- Combina  múltiples fuerzas.

**Tres reglas rigen el flocado:**
- Separación (también conocida como evitación): Conduzca para evitar chocar con sus vecinos.
- Alineación (también conocida como copia): Conduce en la misma dirección que tus vecinos.
- Cohesión (también conocido como centro): Dirígete hacia el centro de tus vecinos (quédate con el grupo).

  ![image](https://github.com/user-attachments/assets/c807d9d1-3b8e-4f4c-ae74-90744ae00534)

#### Boids 

Agentes individuales que simulan el comportamiento colectivo de un grupo, como una bandada de pájaros, un banco de peces o una manada de animales. Cada boid sigue reglas simples, pero cuando actúan juntos, emergen comportamientos complejos y muy realistas.

![image](https://github.com/user-attachments/assets/5435284b-d726-4fd3-a4bb-a7f96a05c86c)

Cada uno de estos agentes sigue tres reglas principales que se codifican en los métodos separate(), align() y cohere().
- Los boids no se chocan, gracias a separate().
- Se agrupan y siguen patrones suaves, gracias a cohere() y align().
- Si arrastras el mouse, agregas más boids al sistema (esto lo hace mouseDragged()).

**1. Identifica las tres reglas:**

*Separación (Separation)*
- Esta función calcula un vector que aleja al boid de los vecinos cercanos para que no se agrupen demasiado.
```js
  separate(boids) {
  let desiredSeparation = 25;
  ...
  for (let i = 0; i < boids.length; i++) {
    let d = p5.Vector.dist(this.position, boids[i].position);
    if (d > 0 && d < desiredSeparation) {
      let diff = p5.Vector.sub(this.position, boids[i].position);
      ...
      steer.add(diff);
      count++;
    }
  }
  ...
  return steer;
}
```

*Alineación (Alignment)*
- Esta función promedia las velocidades de los vecinos cercanos y calcula un vector que empuja al boid a seguir esa dirección.
```js
align(boids) {
  let neighborDistance = 50;
  ...
  for (let i = 0; i < boids.length; i++) {
    let d = p5.Vector.dist(this.position, boids[i].position);
    if (d > 0 && d < neighborDistance) {
      sum.add(boids[i].velocity);
      count++;
    }
  }
  ...
  return steer;
}
```

*Cohesión (Cohesion)*
- Esta función busca el centro de masa de los vecinos cercanos y usa seek() para moverse hacia allá.
```js
cohere(boids) {
  let neighborDistance = 50;
  ...
  for (let i = 0; i < boids.length; i++) {
    let d = p5.Vector.dist(this.position, boids[i].position);
    if (d > 0 && d < neighborDistance) {
      sum.add(boids[i].position);
      count++;
    }
  }
  if (count > 0) {
    sum.div(count);
    return this.seek(sum); // Moverse hacia el centro del grupo
  }
}
```

*¿Dónde se aplican estas tres fuerzas?*
- En el método flock(boids)
```js
flock(boids) {
  let sep = this.separate(boids); // Separación
  let ali = this.align(boids);    // Alineación
  let coh = this.cohere(boids);   // Cohesión

  // Pesos para cada fuerza
  sep.mult(1.5);
  ali.mult(1.0);
  coh.mult(1.0);

  // Aplicar fuerzas
  this.applyForce(sep);
  this.applyForce(ali);
  this.applyForce(coh);
}
```

**2. Explica las Reglas**

*Separación (Separation)*
- Objetivo: Evitar que los boids estén demasiado cerca unos de otros, para que no se amontonen ni se choquen.

*¿Cómo calcula el agente la fuerza de dirección?*
- El boid detecta a sus vecinos más cercanos (dentro de una distancia determinada).
- Si algún vecino está demasiado cerca, calcula un vector que lo aleje de ese vecino.
- Todos esos vectores se suman y se promedian, creando un vector final que apunta lejos del grupo cercano.
- Ese vector se usa como fuerza de separación.

*2. Alineación (Alignment)*
- Objetivo: Hacer que el boid se mueva en la misma dirección que sus vecinos, como si estuvieran "coordinados".

*¿Cómo calcula el agente la fuerza de dirección?*
- El boid busca a los boids que están dentro de un cierto rango de distancia.
- Suma sus velocidades y las promedia.
- Calcula un vector que lo lleva a igualar esa dirección promedio.
- De esta forma, todos tienden a moverse en la misma dirección, como una bandada real.

*3. Cohesión (Cohesion)*
- Objetivo: Mantener a los boids cerca del grupo, para que no se dispersen demasiado.

*¿Cómo calcula el agente la fuerza de dirección?*
- El boid promedia las posiciones de los vecinos cercanos, encontrando el "centro del grupo".
- Luego calcula un vector que lo guía hacia ese centro.
- Esto crea una fuerza que lo empuja a reunirse con los demás, manteniéndolos unidos como grupo.

**3. Identifica parámetros clave**

1. *Radio de percepción (quiénes son los vecinos)*

Estos radios definen el rango de influencia en el que el boid considera a otros como vecinos para cada regla:

- Separación:
```js
let desiredSeparation = 25;
```
  - Está en la función separate(boids).
  - Solo se consideran vecinos muy cercanos para alejarse de ellos.

- Alineación y Cohesión:
```
let neighborDistance = 50;
```
 - Usado en align(boids) y cohere(boids).
 - Define el rango en el que el boid ve a los demás para alinearse o agruparse.

2. *Pesos o multiplicadores de cada regla*

Estos valores determinan cuánta influencia tiene cada comportamiento en el movimiento final:
```js
sep.mult(1.5);
ali.mult(1.0);
coh.mult(1.0);
```
  - Se encuentran en la función flock(boids).
  - sep = separación → más fuerte (1.5)
  - ali = alineación → influencia normal (1.0)
  - coh = cohesión → influencia normal (1.0)

Estos pesos se pueden ajustar para modificar el estilo del movimiento del grupo (por ejemplo, si queremos que se mantengan muy pegados o más dispersos).

3. *Velocidad máxima y fuerza máxima*

Estos controlan el movimiento del boid:

- Velocidad máxima (maxspeed):
```js
this.maxspeed = 3;
```
- Fuerza máxima (de dirección/steering) (maxforce):

```js
this.maxforce = 0.05;
```

Ambos están en el constructor de la clase Boid:
```js
constructor(x, y) {
  ...
  this.maxspeed = 3;
  this.maxforce = 0.05;
}
```
- maxspeed limita qué tan rápido puede ir un boid.
- maxforce limita qué tanto puede cambiar de dirección un boid por frame, para que el movimiento se vea suave.

**4. Modificación**
En la función flock(boids) de la clase Boid, reducimos a cero el peso de la regla de cohesión, comentando o eliminando su influencia:

Antes:
```js
coh.mult(1.0); // La cohesión influye normalmente
```

Después:
```
coh.mult(0); // La cohesión no influye en absoluto
```
- Esto significa que los boids ya no intentan acercarse al centro del grupo ni mantenerse unidos.

*Comportamiento del enjambre:*
- Los boids se dispersan más de lo normal.
- Ya no hay una fuerza que los mantenga agrupados o reunidos.

Aunque todavía:
- Evitan chocar entre sí (por la regla de separación),
- Y siguen moviéndose en direcciones similares (por la regla de alineación),
- El enjambre se ve más caótico y fragmentado.
- Aparecen pequeños grupos sueltos o boids que vuelan solos.
- La estructura compacta y organizada que se observa normalmente desaparece.
