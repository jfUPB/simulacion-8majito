#### Mandala interactivo

***Conceptos Aplicados ⛸️***

1. **Unidad 1: Aleatoriedad.**
- La posición inicial de las partículas (this.pos) se asigna usando random(width) y random(height), lo cual introduce un elemento aleatorio en su colocación.

this.pos = createVector(random(width), random(height));

2. **Unidad 2: Vectores y Motion 101.**
-  Vectores: Las partículas utilizan vectores (createVector) para manejar posición, velocidad, aceleración y dirección. Por ejemplo:
-  
```js
this.pos = createVector(random(width), random(height));
this.target = createVector(x, y);
this.vel = p5.Vector.random2D();
this.acc = createVector();
```

  - Las fuerzas aplicadas (como el efecto de agua) también se calculan con vectores, lo que da vida a las partículas de manera dinámica.
- Motion 101: Se sigue el marco de Motion 101 al trabajar con las tres fases del movimiento:
  - Acumulación de fuerzas (en applyWaterForce() y la atracción hacia el target).
  - Actualización de la aceleración, velocidad y posición.
  - Atenuación de la velocidad con this.vel.mult(0.95) para simular resistencia al movimiento.

4. **Unidad 3: Fuerzas acumulativas, leyes de Newton, resistencia.**
- Fuerzas acumulativas: La aceleración de las partículas (this.acc) acumula las fuerzas que se aplican, como la atracción al target y el efecto repelente del mouse
```js
this.acc.add(force);
this.acc.add(repelForce);
```

- Leyes de Newton:
  - Las partículas siguen las leyes de Newton en su movimiento:
  - La fuerza neta (aceleración) cambia la velocidad.
  - La velocidad cambia la posición:

```js
this.vel.add(this.acc);
this.pos.add(this.vel);
Fricción/resistencia:

```
- La resistencia al movimiento se simula reduciendo la velocidad en cada frame:
this.vel.mult(0.95);

5. **Unidad 5: Sistema de partículas.**

**Creación y actualización:** Las partículas se crean a partir de una imagen (Emitter.createParticles()) y luego se actualizan frame por frame (Emitter.update()).

**Interacción:** Las partículas interactúan entre sí y con el entorno (el mouse), usando fuerzas dinámicas como el efecto de agua.

**Destrucción:** Las partículas desaparecen cuando su vida útil llega a cero:

```js
if (particle.isDead()) {
  this.particles.splice(i, 1);
}
```

(https://editor.p5js.org/Majogc8/sketches/bqP-_i8Y0)
