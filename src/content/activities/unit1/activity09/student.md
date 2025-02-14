[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/9NvfQpKei)
#### El código:

```
let particles = [];  // Array para almacenar las partículas.
const num = 2000;    // Número de partículas a generar.
const noiseScale = 0.01 / 2;  // Escala para modificar el ruido Perlin.
let bias;  // Vector de sesgo para la dirección de movimiento

function setup() {
  createCanvas(1600, 800);
  // Configuramos el modo HSB para trabajar con tonos y saturación.
  // Los valores serán: Hue [0,360], Saturación [0,100], Brillo [0,100] y Alfa [0,100]
  colorMode(HSB, 360, 100, 100, 100);
  
  bias = createVector(0, 0); // Inicialización del vector bias

  // Se crean las partículas con propiedades aleatorias
  for (let i = 0; i < num; i++) {
    particles.push({
      pos: createVector(random(width), random(height)),
      size: random(2, 5),  // Tamaño pequeño
      // Tonos de azul: hue entre 220 y 260, saturación y brillo aleatorios para variar la intensidad
      col: color(random(220, 260), random(50, 100), random(80, 100))
    });
  }
  noStroke();  // No se dibuja borde en las elipses
  clear();
}

function draw() {
  // Fondo negro semitransparente (en HSB: 0,0,0 con alfa 10) para crear un sutil efecto de estela.
  background(0, 0, 0, 10);
  
  // Recorremos todas las partículas
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    
    // Dibujamos la partícula usando su color y tamaño
    fill(p.col);
    ellipse(p.pos.x, p.pos.y, p.size, p.size);
    
    // Calcula el valor del ruido Perlin y obtiene un ángulo
    let n = noise(p.pos.x * noiseScale, p.pos.y * noiseScale, frameCount * noiseScale * noiseScale);
    let a = TAU * n;
    
    // Movimiento base según el ángulo obtenido del ruido
    let moveX = cos(a);
    let moveY = sin(a);
    
    // Se incrementa la probabilidad de moverse en la dirección de la tecla presionada
    moveX += bias.x * 0.9;
    moveY += bias.y * 0.9;
    
    // Se actualiza la posición
    p.pos.x += moveX;
    p.pos.y += moveY;
    
    // Si la partícula sale del lienzo, se reposiciona aleatoriamente
    if (!onScreen(p.pos)) {
      p.pos.x = random(width);
      p.pos.y = random(height);
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
    // Modifica el patrón del ruido Perlin cambiando la semilla
    noiseSeed(millis());
    applyLevyFlight();
  }
}

function keyReleased() {
  bias.set(0, 0); // Restablece el sesgo cuando se suelta la tecla
}

// Verifica si la posición está dentro del lienzo
function onScreen(v) {
  return v.x >= 0 && v.x <= width && v.y >= 0 && v.y <= height;
}

// Función que aplica un vuelo de Lévy a cada partícula
function applyLevyFlight() {
  for (let i = 0; i < num; i++) {
    let p = particles[i];
    let angle = random(TWO_PI);
    let stepSize = pow(random(1), -1.5) * 20; // Distribución de Lévy
    p.pos.x += cos(angle) * stepSize;
    p.pos.y += sin(angle) * stepSize;
    
    if (!onScreen(p.pos)) {
      p.pos.x = random(width);
      p.pos.y = random(height);
    }
  }
}

```
![image](https://github.com/user-attachments/assets/b1f5c341-994e-44c5-8d46-cca71e1cabab)

