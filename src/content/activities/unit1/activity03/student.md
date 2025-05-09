#### Describe el experimento que vas a realizar.
Modificar la particula para que rellene un poco mas de espacio y se ajuste al canva, añadiendo un contador y aumentando un poco la velocidad pero al punto que siga pareciento que deja un rastro coherente

#### ¿Qué pregunta quieres responder con este experimento?
Aproximadamente  en cuantos desplazamientos logra la particula recorrer toda la pantalla

#### ¿Qué resultados esperas obtener?
Como la particula puede recorrer lugares por los que ya pasó, sin haber llegado nisiquiera a algunos rincones del canva asumo que llevara tiempo

#### ¿Qué resultados obtuviste?
aproximaametre a los 1000 desplazamientos a penas la particula llega a recorrer el 75% del canva 

#### ¿Qué aprendiste de este experimento?
Existe la posibilida de que el experimento se logre facilmente ya que la particula puede volver a recorrer espacios que ya llenó y quedarse estancada ahi

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/clMAWJ9sZ)
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
  
  // Vuelve a configurar el modo de dibujo del rectángulo para la partícula
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
    // Genera un vector con dirección aleatoria
    let step = p5.Vector.random2D();
    // Escala el vector para definir la magnitud del paso
    step.mult(8); // Puedes ajustar este valor para cambiar la velocidad
    this.pos.add(step);
    
    // Asegura que la partícula permanezca dentro del canvas
    this.pos.x = constrain(this.pos.x, 0, width);
    this.pos.y = constrain(this.pos.y, 0, height);
  }
  
  display() {
    // Dibuja la partícula como un cuadrado
    stroke(0);
    fill(255, 255, 255);
    rect(this.pos.x, this.pos.y, 8, 8);
  }
}

```

![image](https://github.com/user-attachments/assets/3d5dbf8a-6959-40d3-880f-87816d1bfe57)


