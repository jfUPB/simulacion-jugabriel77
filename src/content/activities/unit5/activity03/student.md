#### Liquid surface

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/mHgzD_fZd)


``` js

// Arreglo de partículas y explosiones
let particles = [];
let explosions = [];
let cols, rows;
let spacing = 20; // Espaciado entre partículas en la malla
let activeParticle = null; // Partícula actualmente seleccionada
let clickStartTime = 0; // Momento en el que se presionó el mouse

function setup() {
  createCanvas(windowWidth, windowHeight);
  cols = floor(width / spacing);
  rows = floor(height / spacing);

  // Crear una malla de partículas
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      let x = i * spacing + spacing / 2;
      let y = j * spacing + spacing / 2;
      particles.push(new Particle(x, y));
    }
  }
}

function draw() {
  background(0);
  
  // Si hay una partícula activa, aplicar el campo gravitacional
  if (activeParticle) {
    let gStrength = map(activeParticle.size, 5, 100, 0.2, 4); // Mapea el tamaño a fuerza de gravedad
    let threshold = activeParticle.size * 10; // Rango de atracción basado en el tamaño de la partícula

    for (let p of particles) {
      if (p !== activeParticle) {
        let d = dist(activeParticle.pos.x, activeParticle.pos.y, p.pos.x, p.pos.y);
        if (d < threshold) {
          let gDir = p5.Vector.sub(activeParticle.pos, p.pos);
          gDir.normalize();
          let forceMag = gStrength * (1 - d / threshold); // Fuerza decreciente según la distancia
          p.vel.add(gDir.mult(forceMag));
        }
      }
    }
  }
  
  // Actualizar y dibujar cada partícula
  for (let p of particles) {
    p.update();
    p.display();
  }
  
  // Actualizar y dibujar cada explosión
  for (let i = explosions.length - 1; i >= 0; i--) {
    explosions[i].update();
    explosions[i].applyForce();
    explosions[i].display();  // Dibujar la onda expansiva
    if (explosions[i].finished()) {
      explosions.splice(i, 1);
    }
  }
}

function mousePressed() {
  // Detectar si el usuario hace clic sobre una partícula
  for (let p of particles) {
    if (dist(mouseX, mouseY, p.pos.x, p.pos.y) < p.size / 2) {
      activeParticle = p;
      clickStartTime = millis(); // Guardar el tiempo de inicio del clic
      break;
    }
  }
}

function mouseReleased() {
  if (activeParticle) {
    // Determinar el alcance y la fuerza de la explosión según el tamaño de la partícula
    let maxRadius = map(activeParticle.size, 5, 100, 50, 1000);
    let impulseStrength = map(activeParticle.size, 5, 100, 0, 5);
    
    // Crear una explosión en la posición de la partícula activa
    let explosion = new Explosion(activeParticle.pos, impulseStrength, maxRadius);
    explosions.push(explosion);
    
    // Reiniciar el tamaño de la partícula seleccionada
    activeParticle.size = 5;
    activeParticle = null;
  }
}

// Clase que representa cada partícula
class Particle {
  constructor(x, y) {
    this.initialPos = createVector(x, y); // Posición original
    this.pos = createVector(x, y); // Posición actual
    this.vel = createVector(0, 0); // Velocidad
    this.acc = createVector(0, 0); // Aceleración
    this.size = 5; // Tamaño inicial
  }
  
  update() {
    // Si la partícula está activa, hacerla crecer mientras se mantiene presionado el mouse
    if (this === activeParticle) {
      let growth = (millis() - clickStartTime) * 0.03;
      this.size = constrain(5 + growth, 5, 100);
    }
    
    // Aplicar una fuerza de resorte para regresar a la posición original
    let springStrength = 0.05;
    let damping = 0.9;
    let force = p5.Vector.sub(this.initialPos, this.pos);
    force.mult(springStrength);
    
    this.acc.add(force);
    this.vel.add(this.acc);
    this.vel.mult(damping);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }
  
  display() {
    noStroke();
    fill(255);
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

// Clase que representa una explosión
class Explosion {
  constructor(center, impulseStrength, maxRadius) {
    this.center = center.copy();
    this.impulseStrength = impulseStrength; // Fuerza del impacto
    this.radius = 0; // Radio inicial de la onda expansiva
    this.speed = 3;  // Velocidad de expansión de la onda
    this.maxRadius = maxRadius; // Máximo alcance de la explosión
  }
  
  update() {
    this.radius += this.speed; // Aumenta el radio con el tiempo
  }
  
  applyForce() {
    let threshold = 10; // Grosor de la onda expansiva
    for (let p of particles) {
      let d = dist(this.center.x, this.center.y, p.pos.x, p.pos.y);
      if (d > this.radius - threshold && d < this.radius + threshold) {
        let dir = p5.Vector.sub(p.pos, this.center).normalize();
        let forceMag = this.impulseStrength * (1 - d / this.maxRadius);
        p.vel.add(dir.mult(forceMag)); // Empujar las partículas
      }
    }
  }
  
  finished() {
    return this.radius > this.maxRadius; // La explosión termina cuando alcanza su radio máximo
  }
  
  display() {
    noFill();
    stroke(255, 100);
    ellipse(this.center.x, this.center.y, this.radius * 2);
  }
}


```
En esta pieza se utilizan conceptos como gravedad, resortes, fuerza de atraccion, fuerza de impacto, ondas; todo esto relacionado deirectamento con el tamaño y el tiempo

![Exp](https://github.com/user-attachments/assets/613efeaf-d5a7-427c-b329-70df094b6981)
