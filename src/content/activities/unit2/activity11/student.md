El codigo por si solo no genera los efectos de mandalas, existe la posibilidad de hacerlo pero eso depende mas de la interacción con el usuario. Al igual de la posibilidad de observar una percepcion 3D como si las particulas orbitaran una esfera

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/Xz_5VwNaL)
[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/seE93LjLK) Version alterna solo para registro (o futuro uso)
```
// Definir una matriz para almacenar los objetos Mover
let movers = [];
let numMovers = 800;

// Variable para almacenar el estado del botón del mouse
let mousePressedState = false;

// Configuración inicial
function setup() {
  createCanvas(800, 800);
  // Crear 20 objetos Mover y agregarlos a la matriz
  for (let i = 0; i < numMovers; i++) {
    movers.push(new Mover());
  }
}

// Bucle de dibujo
function draw() {
  // Limpiar el lienzo en cada iteración
  background(255);
  
  // Verificar si el botón del mouse está presionado
  if (mouseIsPressed && mouseButton === LEFT) {
    mousePressedState = true;
  } else {
    mousePressedState = false;
  }
  
  // Iterar sobre todos los objetos Mover
  for (let mover of movers) {
    // Actualizar y mostrar cada objeto Mover
    mover.update();
    mover.show();
  }
}

// Definición de la clase Mover
class Mover {
  constructor() {
    // Establecer una posición aleatoria dentro del lienzo
    this.position = createVector(random(width), random(height));
    // Asignar una velocidad aleatoria
    this.velocity = p5.Vector.random2D().mult(random(0.5, 3)); // Modificar el rango de velocidad
  }

  // Método para actualizar la posición y la velocidad
  update() {
    // Cambiar la posición aleatoriamente
    this.position.add(this.velocity);
    
    // Verificar si el botón del mouse está presionado
    if (!mousePressedState) {
      // Aplicar cohesión hacia la posición del cursor
      let cohesion = p5.Vector.sub(createVector(mouseX, mouseY), this.position).mult(0.004);
      this.velocity.add(cohesion);
    }
    
    // Limitar la velocidad
    this.velocity.limit(9);
    // Si el objeto sale del lienzo, reubicarlo al otro lado
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  // Método para mostrar el objeto (en este caso, un punto)
  show() {
    noStroke();
    fill(0);
    ellipse(this.position.x, this.position.y, 2); // Tamaño del punto
  }
}


```
![image](https://github.com/user-attachments/assets/629e8bf5-ed51-4808-ab0f-d51beb9a72a2) ![image](https://github.com/user-attachments/assets/b53fcf35-7b17-42b9-a5b7-1ef215024bb2) ![image](https://github.com/user-attachments/assets/8ea63b7f-6f98-4658-a5cb-8aa81a0dea42)



