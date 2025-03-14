#### ¿En qué consiste motion 101?

"Motion 101" es una forma introductoria de entender y simular el movimiento en programación, especialmente en contextos gráficos o de animación.
Consiste en aplicar los principios básicos de la física del movimiento utilizando vectores para representar:
Posición: La ubicación actual del objeto en el espacio.
Velocidad: La tasa de cambio de la posición, es decir, cómo se mueve el objeto.
Aceleración: La tasa de cambio de la velocidad, lo que permite simular efectos de fuerzas (como la gravedad o el empuje).

```
// Declaramos los vectores para posición, velocidad y aceleración
let position;
let velocity;
let acceleration;

function setup() {
  createCanvas(400, 400);
  
  // Inicializamos la posición en el centro de la pantalla
  position = createVector(width / 2, height / 2);
  
  // Asignamos una velocidad inicial (puedes modificarla)
  velocity = createVector(2, 0);
  
  // La aceleración simula la gravedad (hacia abajo)
  acceleration = createVector(0, 0.1);
}

function draw() {
  background(220);
  
  // Actualizamos la velocidad sumándole la aceleración
  velocity.add(acceleration);
  
  // Actualizamos la posición sumándole la velocidad
  position.add(velocity);
  
  // Si el objeto llega a los bordes, invertimos la velocidad para simular un rebote
  if (position.x > width || position.x < 0) {
    velocity.x *= -1;
  }
  
  if (position.y > height || position.y < 0) {
    velocity.y *= -1;
  }
  
  // Dibujamos el objeto (por ejemplo, una esfera)
  fill(50, 100, 200);
  ellipse(position.x, position.y, 48, 48);
}
```
