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

**1.Identifica las tres reglas:**

*1. Separación (Separation)*
- Objetivo: Evitar estar demasiado cerca de otros boids (evitar el hacinamiento).

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
- Esta función calcula un vector que aleja al boid de los vecinos cercanos para que no se agrupen demasiado.
