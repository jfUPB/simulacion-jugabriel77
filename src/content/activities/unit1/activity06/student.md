
[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/-6BhLPKY-)
#### El código:

```
// Número de walkers que se simularán
let numWalkers = 20;
let walkers = [];

// Parámetros para la distribución Lévy
let mu = 1.5;     // Exponente (usualmente entre 1 y 3)
let L_min = 2;    // Longitud mínima del paso
let L_max = 50;   // Longitud máxima (truncamiento para evitar saltos excesivamente grandes)

// Factor de escala para reducir el tamaño de la trayectoria
let scaleFactor = 0.8; // Puedes ajustar este valor entre 0 y 1

function setup() {
  createCanvas(800, 800);
  // Se pinta el fondo una sola vez
  background(220);
  
  // Se crean los walkers en posiciones aleatorias dentro del canvas
  for (let i = 0; i < numWalkers; i++) {
    walkers.push(new Walker(random(width), random(height)));
  }
}

function draw() {
  // Efecto de estela: se dibuja un rectángulo semitransparente sobre el canvas
  noStroke();
  fill(220, 220, 220, 20);
  rect(0, 0, width, height);
  
  // Se actualiza y dibuja cada walker
  for (let walker of walkers) {
    walker.update();
    walker.display();
  }
}

class Walker {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.prevPos = this.pos.copy();
  }
  
  update() {
    // Calcula un salto de Lévy
    let stepLength = levyStep();
    let angle = random(TWO_PI);
    let step = p5.Vector.fromAngle(angle).mult(stepLength);
    
    // Guarda la posición previa y calcula la nueva posición
    this.prevPos = this.pos.copy();
    let newPos = p5.Vector.add(this.pos, step);
    
    // Se comprueba y aplica el rebote en los bordes:
    if (newPos.x < 0) {
      newPos.x = -newPos.x;
    } else if (newPos.x > width) {
      newPos.x = width - (newPos.x - width);
    }
    if (newPos.y < 0) {
      newPos.y = -newPos.y;
    } else if (newPos.y > height) {
      newPos.y = height - (newPos.y - height);
    }
    
    // Actualiza la posición del walker
    this.pos = newPos;
  }
  
  display() {
    push();
      // Se transforma el sistema de coordenadas para rotar y reducir la trayectoria
      // La transformación se aplica respecto al centro del canvas.
      translate(width / 2, height / 2);
      rotate(radians(45)); // Rota 20 grados (ajustable)
      scale(scaleFactor);  // Escala la visualización (0.8 equivale a 80% del tamaño original)
      translate(-width / 2, -height / 2);
      
      // Configuración del trazo
      stroke(0, 150, 255, 200);
      strokeWeight(2);
      line(this.prevPos.x, this.prevPos.y, this.pos.x, this.pos.y);
    pop();
  }
}

// Función que genera un paso según una distribución de Lévy (distribución de potencia)
function levyStep() {
  let u = random();
  // Transformación inversa de la CDF para una ley de potencias:
  // u = 1 - (L_min / x)^(mu - 1)   -->   x = L_min / (1 - u)^(1/(mu - 1))
  let step = L_min / pow(1 - u, 1 / (mu - 1));
  return constrain(step, L_min, L_max);
}

```

![image](https://github.com/user-attachments/assets/10b2d2e6-ec73-48a9-8eab-e05570dc792c)

