#### 1. ¿Qué concepto de oscilación utilizaste como base para tu obra? Describe cómo lo implementaste.

En mi obra, partí del concepto de movimiento oscilatorio amortiguado. Aunque no implementé una oscilación sinusoidal tradicional,
el efecto se logra al generar partículas que se mueven y se desaceleran progresivamente,
simulando un comportamiento similar al de un sistema oscilatorio en el que la amplitud disminuye con el tiempo.
Esto se ve reflejado en el uso del factor de drag (0.95) en cada partícula, lo que provoca que su velocidad disminuya gradualmente,
creando un efecto visual que recuerda al movimiento de oscilación amortiguada.

#### 2. ¿Cómo funciona la interacción en tu obra? Explica cómo el usuario puede modificar la obra en tiempo real.

Particularmente para esra obra sin usuario no existe obra ya que esta surge completamente a partir de la interacción.
Esta forma de interacción permite al usuario modificar el comportamiento visual y la trayectoria de las partículas, 
haciendo que cada trazo sea único y responda de manera inmediata a sus movimientos. 

#### 3.¿Qué desafíos encontraste durante el proceso de creación? ¿Cómo los superaste?

Optimización del rendimiento: Al crear y actualizar múltiples partículas por fotograma,
el rendimiento podía verse afectado. Para solucionarlo, implementé la eliminación de partículas una vez que se agotaba su tiempo de vida,
lo que ayuda a mantener un número controlado de objetos en pantalla.

Lo que genero otro ptoblema y es que si hay 300 particulas en pantalla el hue de color queda estancado hasta que halla disponibilidad en el arreglo.
Al tratar de arreglar este problema nos dimos cuenta que al acercarse al problema por oro angulo, el codigo dejaria de funcionar de la misma forma.


#### 4. ¿Qué aprendiste sobre las oscilaciones y su aplicación en el arte generativo?

Las oscilaciones no solo se tratan de repeticiones regulares, 
sino también de cómo se pueden modular y degradar (como en un sistema amortiguado) para generar efectos visuales interesantes y orgánicos.

El uso de oscilaciones y desaceleraciones en tiempo real añade una capa de complejidad y respuesta directa a la interacción del usuario,
haciendo que la obra se sienta viva y reactiva.

Incorporar parámetros controlados (como el drag y el intervalo de creación de partículas) 
junto con la imprevisibilidad del movimiento del usuario permite explorar un balance entre la estructura matemática de la oscilación y la espontaneidad del arte generativo.


