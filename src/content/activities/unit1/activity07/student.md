#### Explica en tus propias palabras la figura 0.4:
La diferencia radica entre los intervalos de tiempo, es como si la segunda figura fuera un zoom out de la segunda.


#### Explica cómo usaste el ruido Perlin para generar las variaciones:
El valor del ruido Perlin cambia de manera continua y fluida, lo que produce trayectorias orgánicas y menos predecibles para cada partícula, como si siguieran un patrón de viento o de rio.
Primero genera el ruido: Se utiliza la función ```noise()```
Luego con ```noiseScale``` se le asigna el parametro de ruido a las variabes "x" y "y"
Con la formula ```let a = TAU * n;``` se genera un angulo con el ruido
Y para finalizar con las funciones seno y coseno se le da la direccion indicada a la particula

#### El código:

```
let particles = [];  // Crea un array vacío donde se almacenarán las partículas.
const num = 1000;  // Número de partículas a generar.

const noiseScale = 0.01 / 2;  // Escala para modificar el ruido Perlin (hace que el movimiento sea más suave y lento).

function setup() {
  createCanvas(800, 800);  // Crea un lienzo de 800x800 píxeles.
  
  // Inicializa las partículas en posiciones aleatorias dentro del lienzo.
  for (let i = 0; i < num; i++) {
    particles.push(createVector(random(width), random(height)));  // Agrega cada partícula con un vector (x, y) aleatorio.
  }
  
  stroke(255);  // Establece el color del trazo de las partículas (blanco).
  // Intenta descomentar la línea siguiente y comentar la línea de background() en draw.
  stroke(255, 50);  // Establece el trazo con una opacidad del 50%, creando un efecto de estela (trail).
  clear();  // Limpia el lienzo al principio (puedes reemplazarlo por background() para darle color de fondo).
}

function draw() {
  //background(0, 10);  // Se comenta aquí para evitar sobrecargar el lienzo, pero si se descomenta, crea un fondo oscuro.
  
  // Este ciclo recorre todas las partículas.
  for (let i = 0; i < num; i++) {
    let p = particles[i];  // Obtiene la partícula en la posición i.
    point(p.x, p.y);  // Dibuja la partícula en las coordenadas (x, y).
    
     // Aplica el ruido Perlin para determinar la dirección del movimiento.
  let n = noise(p.x * noiseScale, p.y * noiseScale, frameCount * noiseScale * noiseScale);
   let a = TAU * n;  // Genera un ángulo (a) usando el ruido Perlin.
    
    // Mueve la partícula de acuerdo con ese ángulo.
  p.x += cos(a);
   p.y += sin(a);
    
    // Si la partícula se sale de la pantalla, la reposiciona aleatoriamente dentro del lienzo.
   if (!onScreen(p)) {
     p.x = random(width);
     p.y = random(height);
    }
  }
}

// Esta función se llama cuando se suelta el mouse. Cambia la semilla del ruido.
function mouseReleased() {
  noiseSeed(millis());  // Reinicia la semilla del ruido, generando un nuevo patrón.
}

// Esta función verifica si la partícula está dentro del lienzo.
function onScreen(v) {
  return v.x >= 0 && v.x <= width && v.y >= 0 && v.y <= height;
}
```
