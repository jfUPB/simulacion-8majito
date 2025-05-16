el sistema de coordenadas polares para mover un punto (representado por un círculo) alrededor del centro de la pantalla.

#### ¿Cuál es la relación entre r y theta con las posiciones x y y?

a relación entre r y 𝜃 con las posiciones x e y y se basa en la conversión de coordenadas polares a cartesianas.

En coordenadas polares, la posición de un punto en un plano se determina por:

𝑟: La distancia desde el origen hasta el punto.

𝜃: El ángulo medido desde el eje positivo de las x, en radianes.La conversión a coordenadas cartesianas se realiza con las siguientes ecuaciones: 
- x=rcos(θ) ----- x depende del radio y del coseno del ángulo 𝜃. A medida que 𝜃  cambia, el valor de 𝑥 oscila entre −𝑟 y +𝑟.
- y=rsin(θ) ----- y depende del radio y del seno del ángulo 𝜃. A medida que 𝜃 cambia, el valor de 𝑦 oscila entre −𝑟 y +𝑟.

Cada vez que se ejecuta draw(), el valor de theta aumenta en 0.02 radianes:

```js
theta += 0.02;
```
Esto provoca que el punto se mueva en un movimiento circular, ya que tanto 𝑥 como 𝑦 cambian continuamente en función del ángulo 𝜃.

- r determina el tamaño del círculo que sigue el punto.
- theta controla la rotación del punto alrededor del origen.
- x y y son las coordenadas cartesianas del punto en la circunferencia, obtenidas a partir de las funciones trigonométricas seno y coseno.

#### 1. ¿Qué ocurre al ejecutar este código?

El punto ya no se mueve en un círculo grande como antes, sino que apenas se mueve en un círculo muy pequeño alrededor del origen. Esto se debe a que p5.Vector.fromAngle(theta) devuelve un vector unitario, es decir, un vector con magnitud 1, lo que significa que los valores de v.x y v.y siempre estarán en el rango de -1 a 1.

**¿Cómo arreglarlo?**

Podemos multiplicar el vector unitario por r para escalarlo al tamaño correcto.

*p5.Vector.fromAngle(theta)* solo da la dirección (con una magnitud de 1). Para que el punto se mueva en una circunferencia del mismo radio que antes, hay que multiplicar el vector por r.

#### 1. ¿Qué ocurre al ejecutar este código?

En la línea circle(x, y, 48);, la variable y no está definida. En el código pasado, use let v = p5.Vector.fromAngle(theta, r);, lo que significa que ahora x y y están representados dentro del vector v.
Por lo tanto, en la línea del círculo, en lugar de circle(x, y, 48);, debería ser circle(v.x, v.y, 48);.

**¿Por qué ocurre esto?**
- La función p5.Vector.fromAngle(theta, r) devuelve un vector con magnitud r y dirección theta. Esto significa que v.x y v.y ya incluyen la conversión de coordenadas polares a cartesianas, y no es necesario multiplicar por r después.
- p5.Vector.fromAngle(theta, r); ya se encarga de escalar el vector al tamaño correcto, por lo que la línea let v = p5.Vector.fromAngle(theta, r); genera un vector de longitud r, con las coordenadas ya calculadas correctamente.
