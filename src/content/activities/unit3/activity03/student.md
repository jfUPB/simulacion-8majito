*comprender cómo se mueven los objetos alrededor de un lienzo, respondiendo a una variedad de fuerzas ambientales.*

#### Las fuerzas y las leyes del movimiento de 

Una fuerza es un vector que hace que un objeto con masa se acelere.

**Primera ley de Newton**

- Un objeto en reposo permanece en reposo, y un objeto en movimiento permanece en movimiento.
- Un objeto en reposo permanece en reposo, y un objeto en movimiento permanece en movimiento, a una velocidad y dirección constantes, a menos que actúe sobre él una fuerza desequilibrada.
- La velocidad de un objeto permanecerá constante solo en ausencia de fuerzas o solo si las fuerzas que actúan sobre él se cancelan entre sí, lo que significa que la fuerza neta suma cero.
- El vector de velocidad de un objeto permanecerá constante si está en un estado de equilibrio.
  *Mover*
**Segunda ley de Newton** La mas importante en p5.js
- La fuerza es igual a la masa por la aceleración.
- La aceleración es directamente proporcional a la fuerza e inversamente proporcional a la masa. Ejemplo, cuanto más fuerte te presionen, más rápido acelerarás o disminuirás la velocidad (acelerarás). Por otro lado, cuanto más grande eres, ¡menos efectiva es una fuerza para acelerarte!
-  La masa es una medida de la cantidad de materia en un objeto (medida en kilogramos).
- El peso, aunque a menudo se confunde con la masa, es técnicamente la fuerza de gravedad sobre un objeto.

  Ejemplo:
  *mover.applyForce(wind);*
**Tercera ley de Newton**
 - Para cada acción, hay una reacción igual y opuesta.
 - Las fuerzas siempre ocurren en pares. Las dos fuerzas son de igual fuerza pero en direcciones opuestas.
 - Si calcula a llamada que representa una fuerza del objeto A sobre el objeto B, también debe aplicar la fuerza opuesta que el objeto B ejerce sobre el objeto A.

Ejemplo:
*.p5.Vectorfp5.Vector.mult(f, -1)*

La aceleración es igual a la suma de todas las fuerzas dividida por la masa
- La fuerza neta es igual a la masa por la aceleración.
- las fuerzas deben acumularse, o sumarse.

Ejemplos: código que cree una ráfaga de viento al mantener presionado el mouse:acceleration0update()
if (mouseIsPressed) {
  let wind = createVector(0.5, 0);
  mover.applyForce(wind);
}


#### Expliques qué fue lo más te llamó la atención de la unidad

#### ¿Por qué?

#### ¿Qué relación observas entre la unidad anterior y esta en relación con el marco Motion 101?
