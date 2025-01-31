### Example 1.2: Bouncing Ball with Vectors!

#### ¿Cómo funciona la suma dos vectores?

Cada vector tiene dos componentes, una x y una y. Para sumar los dos vectores, sume los componentes x y para crear un nuevo vector. 

![image](https://github.com/user-attachments/assets/15b46ff8-bcf0-47ba-a065-61df42322fa5)

**Propiedades de suma con vectores** 
La suma con vectores sigue las mismas reglas algebraicas que con los números reales.
- Conmutativo: 
3+2=2+3
- Asociativo: 
(3+2)+1=3+(2+1)

En p5.js se usa 

Vectoradd()Vector

La función busca los componentes x e y de los dos vectores y los suma por separado.

 class Vector {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  //{ Una función para agregar otro vector a este vector. Agregue los componentes x y los componentes y por separado.
  add(v) {
    this.x = this.x + v.x;
    this.y = this.y + v.y;
  }
}



#### ¿Por qué esta línea position = position + velocity; no funciona?
