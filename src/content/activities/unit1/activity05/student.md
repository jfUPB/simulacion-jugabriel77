#### Una breve explicación de cómo se refleja la distribución normal en la visualización.
Se utiliza randomGaussian() para generar valores de x y que se suman al centro del canvas, de modo que la mayoría de las figuras se dibujen cerca del centro.
De forma similar, se genera un tamaño para la figura utilizando randomGaussian().

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/Z0pKS_Gk_)
#### El código:

```
function setup() {
  createCanvas(800, 800);
  // Fondo oscuro para resaltar los colores y el efecto de "trail"
  background(20);
  noStroke();
}

function draw() {
  // Dibujar un rectángulo semitransparente que cubra todo el canvas.
  push();
  rectMode(CORNER); // Aseguramos que se dibuje desde la esquina superior izquierda.
  fill(20, 20, 20, 15);
  rect(0, 0, width, height);
  pop();
  
  // Generar posiciones con distribución normal centradas en el canvas
  let x = width / 2 + randomGaussian() * 160;
  let y = height / 2 + randomGaussian() * 160;
  
  // Generar un tamaño aleatorio (positivo) para la figura
  let sizeShape = abs(randomGaussian() * 20) + 10;
  
  // Seleccionar aleatoriamente el tipo de figura: 0 = círculo, 1 = cuadrado, 2 = triángulo
  let shapeType = floor(random(0, 3));
  
  // Generar un color vibrante aleatorio para la figura
  let r = random(50, 255);
  let g = random(50, 255);
  let b = random(50, 255);
  fill(r, g, b, 200);
  
  // Dibujar la figura seleccionada
  if (shapeType === 0) {
    // Círculo
    ellipse(x, y, sizeShape, sizeShape);
  } else if (shapeType === 1) {
    // Cuadrado: encapsulamos el cambio de rectMode para no afectar otros dibujos
    push();
    rectMode(CENTER);
    rect(x, y, sizeShape, sizeShape);
    pop();
  } else {
    // Triángulo equilátero con rotación aleatoria
    push();
    translate(x, y);
    rotate(random(TWO_PI));
    let h = sizeShape * (sqrt(3) / 2);
    triangle(-sizeShape / 2, h / 2, sizeShape / 2, h / 2, 0, -h / 2);
    pop();
  }
}

```

![image](https://github.com/user-attachments/assets/fe2b0978-48b7-4f38-a858-28b4d802b2a3)
