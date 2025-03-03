#### Motion101
El movimiento en el código se aplica mediante los cambios en la posición del objeto mover. 
Esto se hace de forma que el objeto se desplace con una velocidad que depende de su aceleración. 
El calculo de la aceleracíon hace que aumente la velocidad, luego esta se actualiza en cada frame ```this.velocity.add(this.acceleration);```
luego se limita la velocidad ```this.velocity.limit(this.topSpeed);```
y por ultimo se actualiza la posición ```this.position.add(this.velocity);```

