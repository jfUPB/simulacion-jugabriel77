```
let pos; // Vector que almacena la posición actual

function setup() {
  createCanvas(400, 400); // Crea un lienzo de 400x400 píxeles
  background(220);        // Establece un fondo gris claro
  // Inicializa el vector de posición en el centro del lienzo
  pos = createVector(width / 2, height / 2);
  strokeWeight(2);        // Establece el grosor del trazo a 2 píxeles
  stroke(0);              // Establece el color del trazo a negro
}

function draw() {
  // Dibuja un punto en la posición actual
  point(pos.x, pos.y);

  // Genera un número aleatorio entre 0 y 3 para determinar la dirección
  let r = floor(random(4));
  let step; // Vector que representa el paso a dar

  // Según el número aleatorio, se crea un vector de paso en una de las 4 direcciones
  if (r === 0) {
    step = createVector(1, 0);   // Derecha
  } else if (r === 1) {
    step = createVector(-1, 0);  // Izquierda
  } else if (r === 2) {
    step = createVector(0, 1);   // Abajo
  } else {
    step = createVector(0, -1);  // Arriba
  }

  // Actualiza la posición sumando el vector de paso
  pos.add(step);

  // Asegura que la posición se mantenga dentro de los límites del lienzo
  pos.x = constrain(pos.x, 0, width - 1);
  pos.y = constrain(pos.y, 0, height - 1);
}
```
