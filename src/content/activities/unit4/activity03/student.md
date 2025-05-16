#### Vehiculo Direccion

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/COdccsfkd)



``` js

let pos, vel, acc;

function setup() {
  createCanvas(640, 480);
  // Inicia el vehículo en el centro de la pantalla
  pos = createVector(width / 2, height / 2);
  // Comienza en reposo
  vel = createVector(0, 0);
  acc = createVector(0, 0);
}

function draw() {
  background(220);

  // Reinicia la aceleración cada cuadro
  acc.mult(0);

  // Verifica las teclas presionadas para aplicar aceleración en el eje x
  if (keyIsDown(LEFT_ARROW)) {
    acc.x = -0.2;
  } else if (keyIsDown(RIGHT_ARROW)) {
    acc.x = 0.2;
  }

  // Actualiza la velocidad y la posición
  vel.add(acc);
  // Aplica una fricción para simular pérdida de energía
  vel.mult(0.98);
  pos.add(vel);

  // Si el vehículo se sale horizontalmente, lo envuelve a la otra orilla
  if (pos.x > width) pos.x = 0;
  if (pos.x < 0) pos.x = width;

  // Dibuja el vehículo
  push();
  translate(pos.x, pos.y);
  // Calcula el ángulo de la velocidad; si la magnitud es muy pequeña, se asume ángulo 0
  let angle = vel.mag() > 0.1 ? vel.heading() : 0;
  rotate(angle);
  fill(0, 150, 255);
  noStroke();
  // Dibuja un triángulo que apunta hacia la derecha (la "punta" es el frente)
  triangle(20, 0, -10, -10, -10, 10);
  pop();
}
```
![image](https://github.com/user-attachments/assets/c7727164-b29d-4c23-adea-e4210660b59b)

