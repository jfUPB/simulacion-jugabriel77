#### ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas?
En la versión básica de Motion 101 es posible que la aceleración del objeto se establezca directamente a partir de una única fuerza ```this.acceleration = force;```.
Sin embargo, cuando queremos agregar fuerzas acumulativas (varias fuerzas actuando simultáneamente, como la gravedad, el viento, etc.),
debemos sumarlas al vector de aceleración. Es decir, en lugar de asignar la aceleración a una fuerza nueva, se debe acumular cada fuerza usando el método add(): ```this.acceleration.add(force)```;
Esta modificación es necesaria porque, en física, la aceleración total de un objeto es el resultado de la suma de todas las fuerzas que actúan sobre él (según la segunda ley de Newton, F = m · a). 
Además, luego de actualizar la velocidad y la posición, es importante reiniciar la aceleración a cero (usando ```this.acceleration.mult(0)```;)
para que en el siguiente ciclo solo se apliquen las fuerzas nuevas y no se acumulen indefinidamente.

#### Identifica dónde está el Attractor en la simulación. Cambia el color de este.
``` js
display() {
  ellipseMode(CENTER);
  stroke(0);
  if (this.dragging) {
    fill(50);
  } else if (this.rollover) {
    fill(100);
  } else {
    fill(255, 0, 0);  // Nuevo color para el Attractor
  }
  ellipse(this.position.x, this.position.y, this.mass * 2);
}
```

#### Interactuar con el mouse.
``` js
function mousePressed(){
  // Calcula la distancia entre el mouse y el centro del Attractor
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);
  // Si el mouse está dentro del radio (aquí usamos this.mass como aproximación al radio)
  if(d < attractor.mass){
    attractor.dragging = true;
  }
}

function mouseReleased(){
  // Al soltar el mouse, se desactiva el arrastre
  attractor.dragging = false;
}

function mouseMoved(){
  // Detecta si el mouse se encuentra sobre el Attractor
  let d = dist(mouseX, mouseY, attractor.position.x, attractor.position.y);
  attractor.rollover = (d < attractor.mass);
}

function mouseDragged(){
  // Si se está arrastrando el Attractor, actualiza su posición al mouse
  if(attractor.dragging){
    attractor.position.x = mouseX;
    attractor.position.y = mouseY;
  }
}
```
