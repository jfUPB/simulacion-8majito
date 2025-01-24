#### Código del ejemplo

let values = [];

function setup() {
  createCanvas(640, 240);
  background(255);
  noStroke();
}

function draw() {
  // Generar un número aleatorio con distribución normal
  let num = randomGaussian(); // Media 0, desviación estándar 1
  
  // Escalar el número a un rango útil
  let sd = 60; // Desviación estándar
  let mean = width / 2; // Media
  let x = num * sd + mean;

  // Almacenar el valor
  values.push(x);

  // Limpiar la pantalla y graficar
  background(255);
  for (let i = 0; i < values.length; i++) {
    // Asignar un color aleatorio a cada círculo
    let r = random(255);
    let g = random(255);
    let b = random(255);
    fill(r, g, b, 150); // Color aleatorio con algo de transparencia
    
    // Dibujar el círculo
    ellipse(values[i], height / 2, 10, 10);
  }

  // Limitar la cantidad de valores para que no se acumulen infinitamente
  if (values.length > 1000) {
    values.shift();
  }
}


#### Captura de pantalla

![image](https://github.com/user-attachments/assets/e64c9d09-4a96-4d67-8837-016925e85b71)

#### Una breve explicación de cómo se refleja la distribución normal en la visualización

El código usa randomGaussian() para generar números con una distribución normal, centrados en 0. Estos números se escalan para que su media esté en el centro de la pantalla. Las posiciones de las figuras siguen esta distribución, por lo que se agrupan más cerca del centro y se dispersan menos hacia los bordes, creando una visualización de la forma de campana típica de la distribución normal.
