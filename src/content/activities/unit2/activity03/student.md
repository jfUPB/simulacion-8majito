- funciones console.log() y print() para imprimir mensajes en la consola del navegador. 
-  método toString() de la clase p5.Vector para imprimir el vector en la consola.

#### Código 
```js
let position;

function setup() {
    createCanvas(400, 400);  // Crea un lienzo de 400x400 píxeles
    posicion = createVector(6, 9);  // Inicializa el vector con las coordenadas (6, 9)
    playingVector(posicion);  // Modifica las componentes del vector
    noLoop();  // Detiene el loop de draw(), solo se ejecuta una vez
}

function playingVector(v) {
    v.x = 20;  // Modifica la componente x del vector a 20
    v.y = 30;  // Modifica la componente y del vector a 30
}

function draw() {
    background(220);  // Establece un fondo gris
    console.log("Only once");  // Imprime "Only once" en la consola, solo se ve una vez 
}
```
#### ¿Qué resultado esperas obtener?

No lo se, según lo que se dice con las funciones console.log() y print(), si utilizamos esto me imagino que deberá imprimir mensajes en la consola del navegador, que sería Only once.

#### ¿Qué resultado obtuviste?
- Obtuve en la consola un Only once
- Un lienzo gris de 400 x 400
![image](https://github.com/user-attachments/assets/e2d2f2ab-88bb-4a21-858e-163ad7012d24)

#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.

**Paso por valor:** Ocurre cuando se pasan tipos de datos primitivos (números, cadenas, booleanos, etc.). Se crea una copia del valor original y cualquier cambio dentro de la función no afecta la variable original.

```js
let a = 10;
let b = a; // Se copia el valor de a en b
b = 20; // Modificar b NO afecta a a

console.log(a); // 10
console.log(b); // 20
```
**Paso por referencia:** Ocurre cuando pasamos objetos, arreglos o vectores (p5.Vector). En este caso, se pasa una referencia, lo que significa que cualquier modificación dentro de una función afectará el objeto original.

```js
let vectorA = createVector(5, 10);
let vectorB = vectorA; // Ambos apuntan al mismo objeto
vectorB.x = 20; // Modifica el objeto original

console.log(vectorA.toString()); // "p5.Vector { x: 20, y: 10, z: 0 }"
console.log(vectorB.toString()); // "p5.Vector { x: 20, y: 10, z: 0 }"
```
#### ¿Qué tipo de paso se está realizando en el código?

El paso que se realizo en el codigo fue **paso por referencia**. Esto es porque los objetos como los vectores en JavaScript se pasan por referencia, no por valor. Cuando pasas posicion a la función playingVector(), la función modifica directamente las propiedades x y y del vector original, no una copia de él.

```js
let posicion = createVector(6, 9);
playingVector(posicion);
```
#### ¿Qué aprendiste?
- El paso de objetos (como p5.Vector) en JavaScript se hace por referencia, lo que significa que cualquier cambio dentro de una función o modificación directa de ese objeto afecta a la variable original.
- El uso de toString() en un objeto p5.Vector nos permite ver su contenido en formato de texto y verificar cómo se comportan las modificaciones.
- noLoop(); en setup() detiene la ejecución continua de draw(), haciendo que los comandos dentro de draw() se ejecuten solo una vez.
- console.log() y print() nos ayudan a depurar el código imprimiendo valores en la consola del navegador.
