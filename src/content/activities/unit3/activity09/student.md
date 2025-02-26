#### 1. Definir la fuerza (vector de magnitud y dirección).
Especifica qué tipo de fuerza deseas modelar (por ejemplo, gravedad, fricción, viento, etc.).  
La fuerza se describe típicamente con un vector que tiene una magnitud (intensidad) y una dirección.  
```
let gravity = createVector(0, 0.1); // Fuerza de gravedad (apunta hacia abajo)
let wind = createVector(0.1, 0);    // Fuerza del viento (apunta hacia la derecha)
```

#### 2. Aplicar la fuerza al objeto.
La fuerza afectará al objeto de la simulación. Para ello, debes sumar esta fuerza a la aceleración del objeto.
La aceleración es la respuesta del objeto a las fuerzas aplicadas, y la fórmula básica es 𝐹=𝑚⋅𝑎 , donde 𝐹 es la fuerza, 𝑚 es la masa, y 𝑎 es la aceleración.  
```
mover.applyForce(gravity); // Aplica la gravedad al objeto
mover.applyForce(wind);    // Aplica el viento al objeto
```

#### 3. Actualizar la aceleración sumando las fuerzas.
La aceleración del objeto se actualiza sumando la fuerza. Si ya tienes el valor de la fuerza y la masa del objeto, puedes obtener la aceleración. 
Si no, simplemente sumas las fuerzas como lo harías con vectores.  
```
applyForce(force) {
  this.acceleration.add(force);  // Suma la fuerza a la aceleración
}
```

#### 4. Actualizar la velocidad y la posición del objeto.
Después de calcular la aceleración, actualizas la velocidad sumando la aceleración a la velocidad. Luego, la posición del objeto se actualiza sumando la velocidad.
```
this.velocity.add(this.acceleration);  // Actualiza la velocidad
this.position.add(this.velocity);      // Actualiza la posición
```

#### 5. Controlar la magnitud de la fuerza (en caso necesario).
Algunas fuerzas, como la fricción, pueden tener una magnitud que depende de la velocidad o de otros factores. 
Por ejemplo, la fricción puede ser proporcional a la velocidad y actuar en dirección opuesta.  
```
let friction = this.velocity.copy();
friction.normalize();  // Normaliza la dirección
friction.mult(-0.1);   // Aplica una magnitud proporcional a la velocidad
mover.applyForce(friction);  // Aplica la fuerza de fricción
```

#### 6. Resetear la aceleración al final de cada frame.
Al final de cada frame, es necesario resetear la aceleración para evitar que se acumulen fuerzas de frames anteriores.  
```
this.acceleration.mult(0);  // Resetea la aceleración al final del frame
```
