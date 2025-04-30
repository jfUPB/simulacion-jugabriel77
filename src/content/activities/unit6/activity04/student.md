#### Quads
[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/TVUzbJW87)

![image](https://github.com/user-attachments/assets/00f4f168-1717-4b87-881b-5352869530bf)


``` js
let particles = [];  // Array donde se almacenarán las partículas
const num = 1000;    // Número de partículas
const noiseScale = 0.01 / 2;  // Escala para el ruido Perlin
let bias;            // Vector de sesgo
let cellSize = 50;   // Tamaño de cada celda para la cuadrícula espacial
let cols, rows;      // Número de columnas y filas de la cuadrícula
let separationDistance = 25; // Distancia mínima permitida entre partículas
let zOffset = 0;     // Desplazamiento en el eje z para animar el ruido
let speedFactor = 0.003; // Velocidad inicial del cambio de ruido

function setup() {
  createCanvas(1600, 800);
  bias = createVector(0, 0);  // Inicializa el sesgo
  for (let i = 0; i < num; i++) {
    particles.push(createVector(random(width), random(height)));
  }
  stroke(255, 50);  // Líneas más opacas (transparencia del 20%)
  clear();
  
  cols = ceil(width / cellSize);
  rows = ceil(height / cellSize);
}

function draw() {
  background(0);  // Fondo negro
  
  // Movimiento de partículas
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    
    // Hacemos las partículas más grandes y visibles
    noStroke();  // No dibujar borde en las partículas
    fill(255, 200);  // Partículas con más opacidad (80%)
    ellipse(p.x, p.y, 5, 5);  // Dibuja partículas como círculos más grandes
    
    // Calcula el ángulo basado en el ruido Perlin animado
    let n = noise(p.x * noiseScale, p.y * noiseScale, zOffset);
    let a = TAU * n;
    
    let moveX = cos(a);
    let moveY = sin(a);
    
    moveX += bias.x * 0.5;
    moveY += bias.y * 0.5;
    
    p.x += moveX;
    p.y += moveY;
    
    if (!onScreen(p)) {
      p.x = random(width);
      p.y = random(height);
    }
  }
  
  // Actualiza el desplazamiento en el eje z para cambiar el ruido
  zOffset += speedFactor; // Incremento controlado por speedFactor
  
  // Crear cuadrícula
  let grid = new Array(cols * rows);
  for (let i = 0; i < grid.length; i++) {
    grid[i] = [];
  }
  
  // Asignar partículas a la cuadrícula
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    let col = floor(p.x / cellSize);
    let row = floor(p.y / cellSize);
    col = constrain(col, 0, cols - 1);
    row = constrain(row, 0, rows - 1);
    let index = row * cols + col;
    grid[index].push(p);
  }
  
  // Revisión de vecindad y separación
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    let col = floor(p.x / cellSize);
    let row = floor(p.y / cellSize);
    col = constrain(col, 0, cols - 1);
    row = constrain(row, 0, rows - 1);
    
    let candidates = [];
    
    for (let dx = -1; dx <= 1; dx++) {
      for (let dy = -1; dy <= 1; dy++) {
        let nc = col + dx;
        let nr = row + dy;
        if (nc < 0 || nc >= cols || nr < 0 || nr >= rows) continue;
        let index = nr * cols + nc;
        let cellParticles = grid[index];
        for (let candidate of cellParticles) {
          if (candidate === p) continue;
          let dSq = sq(p.x - candidate.x) + sq(p.y - candidate.y);
          let d = sqrt(dSq);
          
          // Separación
          if (d < separationDistance && d > 0) {
            let repel = p5.Vector.sub(p, candidate);
            repel.normalize();
            repel.mult((separationDistance - d) * 0.5);
            p.add(repel);
          }
          
          candidates.push({ candidate: candidate, d: dSq });
        }
      }
    }
    
    // Ordena vecinos por distancia
    candidates.sort((a, b) => a.d - b.d);
    
    // Dibuja líneas con los 3 vecinos más cercanos
    for (let j = 0; j < min(3, candidates.length); j++) {
      let neighbor = candidates[j].candidate;
      stroke(255, 50);  // Líneas con opacidad del 20%
      line(p.x, p.y, neighbor.x, neighbor.y);
    }
  }
}

function keyPressed() {
  if (keyCode === UP_ARROW) {
    bias.set(0, -1);
  } else if (keyCode === DOWN_ARROW) {
    bias.set(0, 1);
  } else if (keyCode === LEFT_ARROW) {
    bias.set(-1, 0);
  } else if (keyCode === RIGHT_ARROW) {
    bias.set(1, 0);
  } else if (keyCode === 32) { // Espaciadora
    applyLevyFlight();
  } else if (key === '+') {
    speedFactor *= 1.1; // Aumentar la velocidad de las partículas
  } else if (key === '-') {
    speedFactor *= 0.9; // Disminuir la velocidad de las partículas
  }
}

function keyReleased() {
  bias.set(0, 0);
}

function onScreen(v) {
  return v.x >= 0 && v.x <= width && v.y >= 0 && v.y <= height;
}

function applyLevyFlight() {
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    let angle = random(TWO_PI);
    let stepSize = pow(random(1), -1.5) * 20;
    p.x += cos(angle) * stepSize;
    p.y += sin(angle) * stepSize;
    
    if (!onScreen(p)) {
      p.x = random(width);
      p.y = random(height);
    }
  }
}


```
