#### Diferencia entre Paso por Valor y Paso por Referencia

**Paso por Valor**
- Se crea una copia independiente del valor original.
- Si la copia cambia, el valor original no se ve afectado.
- Se aplica a tipos de datos primitivos (números, booleanos, strings, etc.).

**Paso por Referencia**
- Se asigna una referencia a la ubicación en memoria del objeto original.
- Si el valor cambia en la copia, también cambia en el original.
- Se aplica a objetos (como p5.Vector, arreglos, y objetos en general).

```js
let friction = this.velocity.copy();  // ✅ PASO POR VALOR (se crea una copia independiente)
let friction = this.velocity;        // ❌ PASO POR REFERENCIA (apunta al mismo objeto)
```
let friction = this.velocity.copy();
- this.velocity.copy() crea un nuevo objeto p5.Vector con los mismos valores.
- friction es ahora un objeto independiente.
- Si modificamos friction, this.velocity no cambiará.

let friction = this.velocity;
- friction no es una copia; simplemente apunta a this.velocity.
- Cualquier cambio en friction también afectará this.velocity, ya que ambos son el mismo objeto en memoria.
- Esto puede causar errores inesperados, como modificar velocity sin intención

#### ¿Qué podría salir mal con let friction = this.velocity

Si queremos calcular la fricción y modificamos friction, en realidad estamos alterando this.velocity, lo que distorsionaría el movimiento del objeto.
