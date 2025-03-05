#### Nieve
[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/mgT7ZnBhy)

![image](https://github.com/user-attachments/assets/5f03ebdd-308a-4e69-afa6-31a62164e64f)

```
let particles = [];
let windSlider;

function setup() {
  createCanvas(800, 600);
  
  // Slider para controlar la fuerza del viento (inicia en el valor medio de 2.5)
  windSlider = createSlider(0, 5, 2.5, 0.1);  // El valor inicial será 2.5 (la mitad de 5)
  windSlider.position(10, height - 30);
  
  // Etiqueta del slider
  let windLabel = createDiv('Wind Force');
  windLabel.position(10, height - 60);
}

function draw() {
  background(0);

  // Obtener la magnitud del viento desde el slider y reducir su influencia
  let windForce = windSlider.value() * 0.1;  // Reducimos la influencia del viento

  // Mostrar las partículas y aplicar la física
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    
    // Aplicar gravedad
    p.applyForce(createVector(0, 0.05)); // Gravedad más suave

    // Aplicar viento dependiendo de la dirección del slider
    if (windSlider.value() < 2.5) {
      // Viento hacia la izquierda
      p.applyForce(createVector(-windForce, 0));
    } else if (windSlider.value() > 2.5) {
      // Viento hacia la derecha
      p.applyForce(createVector(windForce, 0));
    }

    // Actualizar la posición de la partícula
    p.update();
    
    // Dibujar la partícula
    p.display();

    // Si la partícula llega al fondo, la reposicionamos en la parte superior
    if (p.position.y > height) {
      p.reset();
    }

    // Si la partícula se sale por la izquierda, la reposicionamos en el lado derecho
    if (p.position.x < 0) {
      p.position.x = width;
    }

    // Si la partícula se sale por la derecha, la reposicionamos en el lado izquierdo
    if (p.position.x > width) {
      p.position.x = 0;
    }
  }
}

function mousePressed() {
  // Crear una nueva partícula cuando se haga clic
  particles.push(new Snowflake(mouseX, mouseY));
}

// Clase de Partícula (copos de nieve)
class Snowflake {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-2, -1));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
    this.size = random(5, 10); // Tamaño más pequeño para simular copos de nieve
  }
  
  // Aplicar una fuerza a la partícula
  applyForce(force) {
    this.acceleration.add(force);
  }

  // Actualizar la posición y el movimiento de la partícula
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Resetear la aceleración

    // Desvanece la partícula con el tiempo (para que se vea más natural)
    this.lifespan -= 1;
  }

  // Dibujar la partícula como un copo de nieve
  display() {
    noStroke();
    fill(255, this.lifespan); // Blanco con desvanecimiento gradual
    ellipse(this.position.x, this.position.y, this.size, this.size); // Tamaño más pequeño
  }

  // Resetear la partícula a la parte superior de la pantalla
  reset() {
    this.position.y = 0;
    this.position.x = random(width);
    this.velocity = createVector(random(-1, 1), random(1, 3)); // Cambio de dirección al caer
    this.lifespan = 255;
    this.size = random(5, 10); // Tamaño aleatorio para cada copo de nieve
  }
}


```
#### Fuego de colores
[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/lsaf7i30S)
![image](https://github.com/user-attachments/assets/bdd46cb8-d4cf-4ac6-8298-cac4344e1844)
```
let particleSystem;

function setup() {
  createCanvas(800, 800);
  colorMode(HSB);

  particleSystem = new ParticleSystem(
    0,
    createVector(width / 2, height - 50)
  );
}

function draw() {
  background(25);

  // Calcular la fuerza del viento basada en la posición x del mouse
  let dx = map(mouseX, 0, width, -0.2, 0.2);
  let wind = createVector(dx, 0);

  // Aplicar la fuerza del viento y ejecutar el sistema de partículas
  particleSystem.applyForce(wind);
  particleSystem.run();
  
  for (let i = 0; i < 2; i++) {
    particleSystem.addParticle();
  }

  // Dibujar una flecha representando la fuerza del viento
  drawVector(wind, createVector(width / 2, 50), 500);
}

// Función para dibujar un vector como flecha
function drawVector(v, loc, scale) {
  push();
  let arrowSize = 4;
  translate(loc.x, loc.y);
  stroke(255);
  strokeWeight(3);
  rotate(v.heading());

  let length = v.mag() * scale;
  line(0, 0, length, 0);
  line(length, 0, length - arrowSize, arrowSize / 2);
  line(length, 0, length - arrowSize, -arrowSize / 2);
  pop();
}

class ParticleSystem {
  constructor(particleCount, origin) {
    this.particles = [];
    this.origin = origin.copy();
    for (let i = 0; i < particleCount; ++i) {
      this.particles.push(new Particle(this.origin));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let particle = this.particles[i];
      particle.run();

      // Eliminar partículas muertas
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  applyForce(dir) {
    for (let particle of this.particles) {
      particle.applyForce(dir);
    }
  }

  addParticle() {
    this.particles.push(new Particle(this.origin));
  }
}

class Particle {
  constructor(pos) {
    this.loc = pos.copy();
    let xSpeed = randomGaussian() * 0.3;
    let ySpeed = randomGaussian() * 0.3 - 1.0;
    this.velocity = createVector(xSpeed, ySpeed);
    this.acceleration = createVector();
    this.lifespan = 1000.0;
    this.color = color(frameCount % 256, 255, 255);
  }

  run() {
    this.update();
    this.render();
  }

  render() {
    noStroke();
    fill(this.color, this.lifespan);
    ellipse(this.loc.x, this.loc.y, 10, 10);
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  isDead() {
    return this.lifespan <= 0.0;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.loc.add(this.velocity);
    this.lifespan -= 2.5;

    // Detectar colisión con las barreras laterales
    if (this.loc.x <= 10 || this.loc.x >= width - 10) {
      this.velocity.x *= -0.7; // Invertir la velocidad con fricción
      this.loc.x = constrain(this.loc.x, 10, width - 10); // Mantener dentro del lienzo
    }

    // Poner la aceleración en cero
    this.acceleration.mult(0);
  }
}
```

