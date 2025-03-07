#### Explicación de la diferencia entre distribuciones uniformes y no uniformes.
Uniforme: Cada número dentro del intervalo tiene la misma probabilidad de ser seleccionado.Todos los resultados son igualmente probables.
No uniforme: No todos los números tienen la misma probabilidad.
Algunos valores pueden aparecer con mayor frecuencia que otros, de acuerdo con alguna función.

#### Nota:El codigo fue modificado con el envolvimiento toroidal (wrap-around) para que la particula no se quede estancada y se siga visualizando el movimiento

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/IqwwsPeIZ)
#### El código:

```
let particle;
let counter = 0;

function setup() {
  createCanvas(400, 400);
  // Se pinta el fondo una sola vez para poder ver el rastro
  background(1);
  // Configuramos el modo de dibujo de rectángulos para que se centren en la posición
  rectMode(CENTER);
  // Iniciamos la partícula en el centro del canvas
  particle = new Particle(width / 2, height / 2);
}

function draw() {
  // Actualiza la posición de la partícula y aumenta el contador
  particle.walk();
  counter++;
  particle.display();
  
  // Dibuja un rectángulo de fondo para el contador (esto borra parte del rastro en esa zona)
  noStroke();
  fill(220);
  rectMode(CORNER); // Para dibujar el rectángulo en coordenadas estándar
  rect(0, 0, 120, 30);
  
  // Vuelve a configurar el modo de dibujo para la partícula
  rectMode(CENTER);
  
  // Muestra el contador en la esquina superior izquierda
  fill(0);
  textSize(16);
  text("Contador: " + counter, 10, 20);
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
  }
  
  walk() {
    // Genera un ángulo con distribución gaussiana centrada en 0 (favoriza movimiento hacia la derecha)
    // Puedes ajustar el segundo parámetro para controlar la dispersión (a menor valor, menor variación)
    let angle = randomGaussian(0, PI/8);
    
    // Crea un vector unitario a partir del ángulo obtenido
    let step = createVector(cos(angle), sin(angle));
    // Escala el vector para definir la magnitud del paso
    step.mult(8);
    this.pos.add(step);
    
    // En lugar de usar constrain, comprobamos si la partícula se ha salido y la reubicamos al lado opuesto
    if (this.pos.x < 0) {
      this.pos.x = width;
    } else if (this.pos.x > width) {
      this.pos.x = 0;
    }
    
    if (this.pos.y < 0) {
      this.pos.y = height;
    } else if (this.pos.y > height) {
      this.pos.y = 0;
    }
  }
  
  display() {
    // Dibuja la partícula como un cuadrado blanco con borde negro
    stroke(0);
    fill(255);
    rect(this.pos.x, this.pos.y, 8, 8);
  }
}

```

![image](https://github.com/user-attachments/assets/98a253c7-3a08-4470-b9fa-4d5f95b5d5bd)


