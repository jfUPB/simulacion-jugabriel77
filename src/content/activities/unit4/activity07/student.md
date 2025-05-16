#### Osciladores

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/seE93LjLK)



``` js
// Número de líneas
let numLines = 100;
let lines = [];

function setup() {
  createCanvas(800, 800);
  // Inicializar las líneas
  for (let i = 0; i < numLines; i++) {
    lines.push(new DynamicLine(random(width), random(height)));
  }
}

function draw() {
  background(0, 20); // Fondo oscuro con algo de traza

  // Actualizar y mostrar cada línea
  for (let line of lines) {
    line.update();
    line.show();
  }
}

// Clase para crear líneas dinámicas
class DynamicLine {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.maxSpeed = random(2, 5); // Velocidad aleatoria
    this.color = color(random(255), random(255), random(255), 100); // Color aleatorio
  }

  // Método para actualizar la posición y el movimiento de la línea
  update() {
    // Atraer hacia el mouse
    let mousePos = createVector(mouseX, mouseY);
    let force = p5.Vector.sub(mousePos, this.position);
    let distance = force.mag();
    force.normalize();
    let strength = map(distance, 0, width, 0.2, 0.05);
    force.mult(strength);
    this.acceleration = force;

    // Actualizar velocidad y posición
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
  }

  // Método para mostrar la línea en pantalla
  show() {
    // Dibujar la línea desde la posición anterior a la actual
    stroke(this.color);
    strokeWeight(2);
    line(this.position.x, this.position.y, pmouseX, pmouseY); // Línea dinámica
  }
}
```
![descarga (2)](https://github.com/user-attachments/assets/b25b669d-b9bb-4748-b8cc-40d90332c6c7)

