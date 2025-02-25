#### Fricción 🤸
En la simulación de los globos con fricción y viento, hemos modelado diferentes fuerzas utilizando vectores en p5.js

Cuando mezclamos todas estas fuerzas: 
- Gravedad hace que los globos caigan.
- Viento empuja los globos de forma dinámica.
- Fricción ralentiza los globos cuando tocan el suelo.
- Rebote impide que los globos salgan del canvas.

let gravedad = createVector(0, 0.2);
globo.applyForce(gravedad);

Explicación: Se crea un vector de gravedad con dirección hacia abajo (0,0.2) y se aplica a cada globo.

  https://editor.p5js.org/Majogc8/sketches/LIOVEYmlW
