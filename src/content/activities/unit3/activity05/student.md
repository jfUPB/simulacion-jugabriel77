#### ¿Qué problema le ves a este planteamiento?
El problema con el código que has mostrado está en la forma en que se está aplicando la fuerza en el método ```applyForce()```
Actualmente, estás asignando directamente la fuerza a la aceleración en cada llamada a ```applyForce()```
La aceleración sobrescribe la instancia mover cada vez que se llama, lo que no es correcto en una simulación física de movimiento. 
En lugar de reemplazar la aceleración, deberías sumar la fuerza a la aceleración, porque las fuerzas son acumulativas. 
En otras palabras, si varias fuerzas actúan sobre un objeto, su aceleración total es la suma de las aceleraciones que actuan sobre ella como lo dice en el enucniado.

#### ¿Qué solución propones? 
En lugar de ```this.acceleration = force;``` cambiarlo por ```this.acceleration.add(force);```

#### ¿Cómo lo implementarías en p5.js?
 ```
class Mover {
  constructor() {
    this.position = createVector();
    this.velocity = createVector();
    this.acceleration = createVector();
  }

  applyForce(force) {
    this.acceleration.add(force); // Suma la fuerza a la aceleración
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);  // Reinicia la aceleración para no acumular fuerzas en el siguiente frame
  }

  show() {
    ellipse(this.position.x, this.position.y, 48, 48);
  }
}
 ```
