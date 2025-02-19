*comprender cómo se mueven los objetos alrededor de un lienzo, respondiendo a una variedad de fuerzas ambientales.*

#### Las fuerzas y las leyes del movimiento de 

Una fuerza es un vector que hace que un objeto con masa se acelere.

**Primera ley de Newton**
- Un objeto en reposo permanece en reposo, y un objeto en movimiento permanece en movimiento.
- Un objeto en reposo permanece en reposo, y un objeto en movimiento permanece en movimiento, a una velocidad y dirección constantes, a menos que actúe sobre él una fuerza desequilibrada.
- La velocidad de un objeto permanecerá constante solo en ausencia de fuerzas o solo si las fuerzas que actúan sobre él se cancelan entre sí, lo que significa que la fuerza neta suma cero.
- El vector de velocidad de un objeto permanecerá constante si está en un estado de equilibrio.
  *Mover*
  - Todas las fuerzas que actúan sobre un objeto suman cero, el objeto experimenta un estado de equilibrio (es decir, sin aceleración).
    
**Segunda ley de Newton** La mas importante en p5.js
- *La fuerza es igual a la masa por la aceleración------ la aceleración es igual a la suma de todas las fuerzas dividida por la masa.* 
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

Datos: 
- La masa es un escalar, no un vector, ya que es solo un número que describe la cantidad de materia en un objeto.
- Crear 2 objetos 
```js
// A large mover on the left side of the canvas
let moverA = new Mover(100, 30, 10);
// A smaller mover on the right side of the canvas
let moverB = new Mover(400, 30, 2);
```
- Cuando son dos obejtos o masas tener en cuenta que cada operación en el código se escribe dos veces, una para y otra para
- Gravedad escalada por la masa
  
**Fricción**
- Fuerza de fricción: Comencemos con la fricción y sigamos los pasos anteriores. Cada vez que dos superficies entran en contacto, experimentan fricción. La fricción es una fuerza disipadora, lo que significa que hace que la energía cinética de un objeto se convierta en otra forma, dando la impresión de pérdida o disipación.
  
![image](https://github.com/user-attachments/assets/a1f10129-0da9-409b-863a-c9f0c24fcf66)

#### Expliques qué fue lo más te llamó la atención de la unidad

Lo que más me llamó la atención de la unidad fue la forma en que diferentes fuerzas, como la gravedad, la fricción y el viento, afectan el movimiento de los objetos en función de su masa. Me pareció muy interesante ver cómo objetos con distinta masa rebotaban de manera diferente y cómo ciertas superficies, como las viscosas, podían modificar drásticamente su comportamiento. También me sorprendió cómo la simulación de fuerzas en p5.js permite crear interacciones realistas con pocos cálculos, haciendo que los objetos respondan de manera convincente a su entorno.

#### ¿Por qué?

Además, al aplicar estos conceptos en p5.js, se pueden crear animaciones más dinámicas y creíbles. Entender cómo las fuerzas afectan los objetos también me ayuda en animación, ya que puedo hacer que los movimientos de los personajes y su interacción con el entorno sean más realistas.

#### ¿Qué relación observas entre la unidad anterior y esta en relación con el marco Motion 101?

Veo que ambas unidades trabajan el movimiento, pero desde perspectivas diferentes. En la unidad anterior, con Motion 101, aprendimos sobre interpolación y cómo suavizar transiciones usando lerp() y lerpColor(), lo que permite crear movimientos fluidos y controlados. En esta unidad, se introduce un enfoque más basado en las leyes físicas, donde el movimiento no solo es interpolado sino que responde a fuerzas externas como la gravedad y la fricción. Ambas unidades se complementan, ya que Motion 101 da herramientas para controlar el movimiento de forma predecible, mientras que la aplicación de fuerzas permite que las simulaciones sean más dinámicas e interactivas.
