```
let t = 0;  // Factor de interpolación que va de 0 a 1
let direction = 1; // Controla la dirección de la interpolación (hacia adelante o hacia atrás)

function setup() 
{
    createCanvas(200, 200); // Crea un lienzo de 200x200 píxeles
}

function draw() 
{
    background(200); // Establece un fondo gris claro

    // Definición de vectores para las flechas
    let v0 = createVector(50, 50); // Punto base para las flechas
    let v1 = createVector(100, 0);   // Flecha roja
    let v2 = createVector(0, 100);   // Flecha azul
    let v3 = p5.Vector.lerp(v1, v2, t); // Vector interpolado entre v1 y v2
    let v4 = p5.Vector.sub(v2, v1); // Flecha verde 1 (diferencia entre v2 y v1)
    let v5 = createVector(150, 50); // Flecha verde 2

    // Definición de colores
    let c1 = color(255, 0, 0); // Rojo
    let c2 = color(0, 0, 255); // Azul
    let c3 = lerpColor(c1, c2, t); // Color interpolado entre rojo y azul

    // Dibujo de flechas
    drawArrow(v0, v1, 'red');  // Dibuja la flecha roja
    drawArrow(v0, v2, 'blue'); // Dibuja la flecha azul
    drawArrow(v0, v3, c3);     // Dibuja la flecha interpolada
    drawArrow(v5, v4, 'green'); // Dibuja la flecha verde

    // Animación de la interpolación
    t += 0.02 * direction; // Incrementa el factor de interpolación
    if (t >= 1 || t <= 0) {
        direction *= -1; // Invierte la dirección al alcanzar los límites
    }
}

function drawArrow(base, vec, myColor) 
{
    push(); // Guarda el estado actual
    stroke(myColor); // Establece el color del trazo
    strokeWeight(3); // Establece el grosor del trazo
    fill(myColor); // Establece el color de relleno
    translate(base.x, base.y); // Traslada al punto base
    line(0, 0, vec.x, vec.y); // Dibuja la línea de la flecha
    rotate(vec.heading()); // Rota el sistema de coordenadas según la dirección del vector
    let arrowSize = 7; // Tamaño de la punta de la flecha
    translate(vec.mag() - arrowSize, 0); // Mueve a la posición de la punta de la flecha
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0); // Dibuja la punta de la flecha
    pop(); // Restaura el estado anterior
}
```
