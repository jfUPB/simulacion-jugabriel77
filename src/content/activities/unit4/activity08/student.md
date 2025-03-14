#### Ola

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/-f5CcQwoZ0)



``` js
let angle = 0;
let angleVelocity = 0.2;
let amplitude = 100;

function setup() {
  createCanvas(800, 400);
}

function draw() {
  background(255);
  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  let currentAngle = angle; // Se mantiene un ángulo base

  for (let x = 0; x <= width; x += 24) {
    // Calcular la posición y según la amplitud y el seno del ángulo
    let y = amplitude * sin(currentAngle);
    
    // Dibujar el círculo en la posición (x, y)
    circle(x, y + height / 2, 48);
    
    // Incrementar el ángulo para la siguiente posición en x
    currentAngle += angleVelocity;
  }
  
  // Avanzar el ángulo base para animar el movimiento de la ola
  angle += 0.05;
}

```
![image](https://github.com/user-attachments/assets/0b1fa5f3-26b8-43bf-b348-826d2ebf706c)

