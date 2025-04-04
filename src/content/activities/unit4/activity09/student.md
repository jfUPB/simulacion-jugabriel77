#### Dos resortes

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/mcZNFQFY9)



``` js
// Clase Bob: representa una masa que se mueve bajo fuerzas.
class Bob {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = 20;
    this.dragging = false;
    this.rollover = false;
  }
  
  applyForce(force) {
    // F = m * a  =>  a = F/m
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }
  
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    // Reinicia la aceleración para el siguiente cuadro
    this.acceleration.mult(0);
  }
  
  show() {
    fill(127);
    noStroke();
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
  
  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
    }
  }
  
  stopDragging() {
    this.dragging = false;
  }
  
  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.set(mx, my);
    }
  }
}

// Clase Spring: simula un resorte que conecta un ancla con un objeto Bob.
class Spring {
  constructor(x, y, restLength) {
    this.position = createVector(x, y);
    this.restLength = restLength;
  }
  
  // Aplica la fuerza del resorte al Bob conectado
  connect(bob) {
    // Calcula el vector entre el Bob y el ancla
    let force = p5.Vector.sub(bob.position, this.position);
    let currentLength = force.mag();
    let x = currentLength - this.restLength; // extensión o compresión
    let k = 0.1; // constante del resorte (ajustable)
    force.normalize();
    // La fuerza se aplica en sentido contrario a la extensión o compresión
    force.mult(-1 * k * x);
    bob.applyForce(force);
  }
  
  // Constrain la distancia entre el ancla y el Bob
  constrainLength(bob, minLen, maxLen) {
    let d = p5.Vector.dist(this.position, bob.position);
    if (d < minLen) {
      let diff = minLen - d;
      let dir = p5.Vector.sub(bob.position, this.position).normalize();
      bob.position.add(dir.mult(diff));
    } else if (d > maxLen) {
      let diff = d - maxLen;
      let dir = p5.Vector.sub(bob.position, this.position).normalize();
      bob.position.sub(dir.mult(diff));
    }
  }
  
  // Dibuja una línea representando el resorte
  showLine(bob) {
    stroke(0);
    line(this.position.x, this.position.y, bob.position.x, bob.position.y);
  }
  
  // Dibuja el ancla del resorte
  show() {
    fill(200);
    noStroke();
    ellipse(this.position.x, this.position.y, 16);
  }
}

// Variables globales para los bobs y los resortes.
let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 240);
  
  // Primer resorte: anclado en un punto fijo (parte superior) y conectado a bob1.
  spring1 = new Spring(width / 2, 10, 100);
  bob1 = new Bob(width / 2, 100);
  
  // Segundo resorte: ancla se actualizará para que siempre esté en bob1.
  spring2 = new Spring(bob1.position.x, bob1.position.y, 100);
  bob2 = new Bob(width / 2, 200);
}

function draw() {
  background(255);
  
  // Fuerza de gravedad aplicada a ambos bobs.
  let gravity = createVector(0, 2);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);
  
  // Actualiza la posición y velocidad de ambos bobs.
  bob1.update();
  bob2.update();
  
  // Permite arrastrar los bobs con el mouse.
  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);
  
  // --- Resorte 1 ---
  // Conecta el primer resorte con bob1.
  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);
  spring1.showLine(bob1);
  
  // --- Resorte 2 ---
  // Actualiza la posición del ancla del segundo resorte para que sea la posición actual de bob1.
  spring2.position = bob1.position.copy();
  spring2.connect(bob2);
  spring2.constrainLength(bob2, 30, 200);
  spring2.showLine(bob2);
  
  // Dibuja los bobs.
  bob1.show();
  bob2.show();
  
  // Dibuja los anclas de los resortes.
  spring1.show();
  spring2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}


```
