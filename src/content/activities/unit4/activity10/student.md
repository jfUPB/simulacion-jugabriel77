#### Doble Pendulo (conectados en serie)

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/-f5CcQwoZ0)



``` js


class DoublePendulum {
  constructor(x, y, r1, r2, m1, m2) {
    // Posición fija del pivote (origen del primer péndulo)
    this.pivot = createVector(x, y);
    // Longitudes de los brazos
    this.r1 = r1;
    this.r2 = r2;
    // Masas (que también determinan el tamaño de las bolas)
    this.m1 = m1;
    this.m2 = m2;
    // Ángulos iniciales (medidos desde la vertical)
    this.a1 = PI / 4;
    this.a2 = PI / 4;
    // Velocidades angulares
    this.a1_v = 0;
    this.a2_v = 0;
    // Aceleraciones angulares
    this.a1_a = 0;
    this.a2_a = 0;
    // Amortiguación para simular fricción
    this.damping = 0.995;
    // Gravedad
    this.g = 0.4;
    
    // Variables para la interacción con el mouse
    this.draggingBob1 = false;
    this.draggingBob2 = false;
  }
  
  update() {
    // Actualiza el primer péndulo solo si no se está arrastrando
    if (!this.draggingBob1) {
      let num1 = -this.g * (2 * this.m1 + this.m2) * sin(this.a1);
      let num2 = -this.m2 * this.g * sin(this.a1 - 2 * this.a2);
      let num3 = -2 * sin(this.a1 - this.a2) * this.m2;
      let num4 = this.a2_v * this.a2_v * this.r2 + this.a1_v * this.a1_v * this.r1 * cos(this.a1 - this.a2);
      let den = this.r1 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.a1 - 2 * this.a2));
      this.a1_a = (num1 + num2 + num3 * num4) / den;
      
      this.a1_v += this.a1_a;
      this.a1 += this.a1_v;
      
      this.a1_v *= this.damping;
    } else {
      // Si se está arrastrando, se anula la velocidad y aceleración
      this.a1_a = 0;
      this.a1_v = 0;
    }
    
    // Actualiza el segundo péndulo solo si no se está arrastrando
    if (!this.draggingBob2) {
      let num1 = 2 * sin(this.a1 - this.a2);
      let num2 = this.a1_v * this.a1_v * this.r1 * (this.m1 + this.m2);
      let num3 = this.g * (this.m1 + this.m2) * cos(this.a1);
      let num4 = this.a2_v * this.a2_v * this.r2 * this.m2 * cos(this.a1 - this.a2);
      let den = this.r2 * (2 * this.m1 + this.m2 - this.m2 * cos(2 * this.a1 - 2 * this.a2));
      this.a2_a = (num1 * (num2 + num3 + num4)) / den;
      
      this.a2_v += this.a2_a;
      this.a2 += this.a2_v;
      
      this.a2_v *= this.damping;
    } else {
      this.a2_a = 0;
      this.a2_v = 0;
    }
  }
  
  show() {
    // Calcula la posición del primer péndulo (bob1)
    let bob1 = createVector(this.r1 * sin(this.a1), this.r1 * cos(this.a1));
    bob1.add(this.pivot);
    
    // Calcula la posición del segundo péndulo (bob2) a partir de bob1
    let bob2 = createVector(this.r2 * sin(this.a2), this.r2 * cos(this.a2));
    bob2.add(bob1);
    
    stroke(0);
    strokeWeight(2);
    // Dibuja el primer brazo
    line(this.pivot.x, this.pivot.y, bob1.x, bob1.y);
    // Dibuja el segundo brazo
    line(bob1.x, bob1.y, bob2.x, bob2.y);
    
    fill(127);
    // Dibuja las masas con diámetros proporcionales a las masas definidas
    circle(bob1.x, bob1.y, this.m1 * 2);
    circle(bob2.x, bob2.y, this.m2 * 2);
  }
}

let dp;

function setup() {
  createCanvas(640, 480);
  // Crea el doble péndulo:
  // - Pivote en (width/2, 100)
  // - Longitudes de 125 píxeles para cada brazo
  // - Masas de 20 (determinan el tamaño de las bolas)
  dp = new DoublePendulum(width / 2, 100, 125, 125, 20, 20);
}

function draw() {
  background(255);
  dp.update();
  dp.show();
}

// Función auxiliar para obtener las posiciones actuales de los "bobs"
function getBobPositions() {
  let bob1 = createVector(dp.r1 * sin(dp.a1), dp.r1 * cos(dp.a1));
  bob1.add(dp.pivot);
  let bob2 = createVector(dp.r2 * sin(dp.a2), dp.r2 * cos(dp.a2));
  bob2.add(bob1);
  return { bob1, bob2 };
}

function mousePressed() {
  let { bob1, bob2 } = getBobPositions();
  // Si el clic está cerca del segundo péndulo, activar arrastre de bob2
  if (dist(mouseX, mouseY, bob2.x, bob2.y) < dp.m2) {
    dp.draggingBob2 = true;
  }
  // Si no, si está cerca del primer péndulo, activar arrastre de bob1
  else if (dist(mouseX, mouseY, bob1.x, bob1.y) < dp.m1) {
    dp.draggingBob1 = true;
  }
}

function mouseDragged() {
  // Si se está arrastrando el primer péndulo, actualizar el ángulo a partir del pivote
  if (dp.draggingBob1) {
    // Calcula el ángulo entre el pivote y la posición del mouse
    dp.a1 = atan2(mouseX - dp.pivot.x, mouseY - dp.pivot.y);
  }
  // Si se está arrastrando el segundo péndulo, actualizar el ángulo a partir de bob1
  if (dp.draggingBob2) {
    let bob1 = createVector(dp.r1 * sin(dp.a1), dp.r1 * cos(dp.a1));
    bob1.add(dp.pivot);
    dp.a2 = atan2(mouseX - bob1.x, mouseY - bob1.y);
  }
}

function mouseReleased() {
  // Al soltar el mouse, se desactivan ambos arrastres
  dp.draggingBob1 = false;
  dp.draggingBob2 = false;
}

```
![image](https://github.com/user-attachments/assets/c874e765-442a-4e70-b3ab-b8c8386d1cfe)

