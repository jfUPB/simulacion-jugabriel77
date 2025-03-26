``` js

// Variable 't' actúa como factor de interpolación entre vectores
let t = 0;  
// Variable 'direction' controla la dirección en la que avanza la interpolación (incremento o decremento)
let direction = 1; 

function setup() {
    createCanvas(800, 800); // Crea un lienzo de 800x800 píxeles
}

function draw() {
    background(200); // Pinta el fondo con un color gris claro

    // 'base' es un vector cuya posición sigue al cursor del mouse
    let base = createVector(mouseX, mouseY); 
    // 'scaleFactor' es un factor de escala que varía según la posición horizontal del mouse
    let scaleFactor = map(mouseX, 0, width, 0.5, 2); 

    // Se crean dos vectores base, escalados por 'scaleFactor'
    let v1 = createVector(100, 0).mult(scaleFactor);   // Flecha roja (v1), apuntando a la derecha
    let v2 = createVector(0, 100).mult(scaleFactor);   // Flecha azul (v2), apuntando hacia abajo

    // 'v3' es el vector interpolado entre 'v1' y 'v2', utilizando 't' como factor de interpolación
    let v3 = p5.Vector.lerp(v1, v2, t); 
    // 'v4' es la diferencia entre 'v2' y 'v1', lo que da lugar a otro vector (para la flecha verde)
    let v4 = p5.Vector.sub(v2, v1); 
    // 'v5' es un vector similar a 'v1' para ajustar la posición base de la flecha verde
    let v5 = createVector(100, 0).mult(scaleFactor); 

    // Se definen dos colores base para la interpolación de color
    let c1 = color(255, 0, 0); // Rojo
    let c2 = color(0, 0, 255); // Azul
    // 'c3' es el color resultante de la interpolación entre 'c1' y 'c2' usando el factor 't'
    let c3 = lerpColor(c1, c2, t); 

    // Dibuja las flechas en el lienzo:
    drawArrow(base, v1, 'red');  // Flecha roja
    drawArrow(base, v2, 'blue'); // Flecha azul
    drawArrow(base, v3, c3);     // Flecha interpolada (color cambia gradualmente de rojo a azul)
    // Flecha verde: se ajusta la base sumándole 'v5' y se dibuja el vector 'v4'
    drawArrow(base.copy().add(v5), v4, 'green'); 

    // Actualiza el factor de interpolación para animar el cambio entre vectores
    t += 0.02 * direction;
    // Si 't' llega a 1 o 0, se invierte la dirección para que la animación sea oscilatoria
    if (t >= 1 || t <= 0) {
        direction *= -1; 
    }
}

// Función que dibuja una flecha a partir de una base, un vector y un color
function drawArrow(base, vec, myColor) {
    push(); // Guarda el estado actual del lienzo
    stroke(myColor); // Establece el color del trazo
    strokeWeight(3); // Define el grosor del trazo
    fill(myColor); // Establece el color de relleno para la punta de la flecha
    translate(base.x, base.y); // Traslada el origen a la posición 'base'
    line(0, 0, vec.x, vec.y); // Dibuja la línea de la flecha
    rotate(vec.heading()); // Rota el sistema de coordenadas para orientar la flecha en la dirección del vector
    let arrowSize = 7; // Tamaño de la punta de la flecha
    // Traslada la posición al final de la línea (ajustado para el tamaño de la punta)
    translate(vec.mag() - arrowSize, 0);
    // Dibuja la punta de la flecha (un triángulo)
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop(); // Restaura el estado previo del lienzo
}
```
