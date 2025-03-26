#### Particulas Conectadas

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/Ypr5MbtNz)

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/yFxU9lGw8) (Iteracion 2.0)


``` js
// Array de objetos "Path", cada uno contiene un array de partículas
let paths = [];

// Cuántos fotogramas deben pasar antes de crear una nueva partícula
let framesBetweenParticles = 2;
let nextParticleFrame = 0;

// Ubicación de la última partícula creada
let previousParticlePosition;

// Cuánto tarda una partícula en desvanecerse (en fotogramas)
let particleFadeFrames = 300;

function setup() {
  createCanvas(1600, 800); // Crea un lienzo de 720x400 píxeles
  colorMode(HSB); // Usa el modo de color HSB (Tono, Saturación, Brillo)

  // Se inicia con un vector por defecto que guarda la posición
  // de la última partícula creada
  previousParticlePosition = createVector();

  describe(
    'Cuando el cursor se arrastra sobre el fondo negro, se dibuja un patrón de círculos multicolores con borde blanco, conectados por líneas blancas. Los círculos y las líneas se desvanecen con el tiempo.'
  );
}

function draw() {
  background(0); // Fondo negro

  // Actualiza y dibuja todos los caminos (paths)
  for (let path of paths) {
    path.update();
    path.display();
  }
}

// Crea un nuevo camino cuando se presiona el mouse
function mousePressed() {
  nextParticleFrame = frameCount; // Establece el tiempo del siguiente fotograma de partícula
  paths.push(new Path()); // Agrega un nuevo camino

  // Reinicia la posición anterior de la partícula al mouse
  // para que la primera partícula del camino tenga velocidad cero
  previousParticlePosition.set(mouseX, mouseY);
  createParticle(); // Crea la primera partícula
}

// Agrega partículas cuando el mouse se arrastra
function mouseDragged() {
  // Si es momento de agregar una nueva partícula
  if (frameCount >= nextParticleFrame) {
    createParticle();
  }
}

function createParticle() {
  // Obtiene la posición actual del mouse
  let mousePosition = createVector(mouseX, mouseY);

  // Calcula la velocidad de la nueva partícula en función del movimiento del mouse
  let velocity = p5.Vector.sub(mousePosition, previousParticlePosition);
  velocity.mult(0.05); // Reduce la velocidad para suavizar el movimiento

  // Agrega una nueva partícula al último camino creado
  let lastPath = paths[paths.length - 1];
  lastPath.addParticle(mousePosition, velocity);

  // Programa la siguiente partícula
  nextParticleFrame = frameCount + framesBetweenParticles;

  // Guarda la posición actual del mouse como la última posición registrada
  previousParticlePosition.set(mouseX, mouseY);
}

// Clase Path que representa un camino formado por partículas
class Path {
  constructor() {
    this.particles = []; // Lista de partículas en el camino
  }

  addParticle(position, velocity) {
    // Crea una nueva partícula con una posición, velocidad y color específico
    let particleHue = (this.particles.length * 30) % 360; // Define un tono basado en la cantidad de partículas
    //console.log(`this.particles.length: ${this.particles.length}`);
    this.particles.push(new Particle(position, velocity, particleHue));
  }

  // Actualiza todas las partículas en el camino
  update() {
    for (let particle of this.particles) {
      particle.update();
    }
  }

  // Dibuja una línea entre dos partículas
  connectParticles(particleA, particleB) {
    let opacity = particleA.framesRemaining / particleFadeFrames; // Opacidad basada en el tiempo restante de la partícula
    stroke(255, opacity); // Línea blanca con opacidad variable
    line(
      particleA.position.x,
      particleA.position.y,
      particleB.position.x,
      particleB.position.y
    );
  }

  // Dibuja el camino con sus partículas y líneas de conexión
  display() {
    // Se recorre la lista de partículas en orden inverso para facilitar la eliminación de las partículas que se desvanecen
    for (let i = this.particles.length - 1; i >= 0; i -= 1) {
      // Elimina la partícula si ya no le quedan fotogramas de vida
      if (this.particles[i].framesRemaining <= 0) {
        this.particles.splice(i, 1);
      } else {
        // Dibuja la partícula
        this.particles[i].display();

        // Si hay una partícula siguiente en la lista
        if (i < this.particles.length - 1) {
          // Conéctala con una línea a la siguiente partícula
          this.connectParticles(this.particles[i], this.particles[i + 1]);
        }
      }
    }
  }
}

// Clase Particle que representa una partícula individual
class Particle {
  constructor(position, velocity, hue) {
    this.position = position.copy(); // Guarda la posición inicial
    this.velocity = velocity.copy(); // Guarda la velocidad inicial
    this.hue = hue; // Color de la partícula
    this.drag = 0.95; // Factor de arrastre para desacelerar la partícula
    this.framesRemaining = particleFadeFrames; // Tiempo antes de desaparecer
  }

  update() {
    // Mueve la partícula según su velocidad
    this.position.add(this.velocity);

    // Reduce la velocidad gradualmente
    this.velocity.mult(this.drag);

    // Reduce el tiempo de vida de la partícula
    this.framesRemaining -= 1;
  }

  // Dibuja la partícula en pantalla
  display() {
    let opacity = this.framesRemaining / particleFadeFrames; // Calcula la opacidad en función del tiempo restante
    noStroke(); // Sin borde
    fill(this.hue, 80, 90, opacity); // Color de la partícula con opacidad variable
    circle(this.position.x, this.position.y, 24); // Dibuja un círculo en la posición de la partícula
  }
}


```

![descarga (5)](https://github.com/user-attachments/assets/87e1333a-08c0-409f-9288-217b72dc6709)

