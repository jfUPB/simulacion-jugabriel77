#### Comprensión de Matter.js
Después de trabajar con este proyecto, tengo una comprensión funcional de los fundamentos de Matter.js: sé cómo implementar librerias, cuerpos físicos, y manejar colisiones.
Me siento cómodo manipulando propiedades físicas como gravedad, restitución o fricción. Sin embargo, aún encuentro confusas algunas áreas, como el uso avanzado de cuerpos compuestos,
vértices personalizados (Bodies.fromVertices()), y el manejo detallado de colisiones múltiples y contactos complejos. 
También el control fino de fuerzas y la sincronización precisa entre lógica física y visual puede ser desafiante.

#### Éxito en la aplicación creativa
Estoy satisfecho con el resultado de la animación. Logré una representación clara del concepto de “linterna”: la "T" se comporta como un foco que ilumina las letras al tocarlas o apuntarlas,
lo que comunica el significado de la palabra de forma visual e interactiva. Aunque la ejecución es efectiva, hay margen de mejora en la precisión de la iluminación y en la fidelidad visual de las formas.

#### Desafío técnico vs. creativo
El mayor desafío fue técnico: entender cómo usar Matter.js para lograr el comportamiento físico deseado (especialmente el movimiento controlado de la "T" y la iluminación direccional).
Aunque lo creativo también fue retador, la idea de usar la "T" como linterna surgió de forma relativamente natural una vez entendidas las posibilidades físicas. 
El aspecto técnico requirió más tiempo de prueba y error, con un gran campo de mejora.

#### Resolución de problemas
Un problema específico fue cómo detectar que la "T" estaba “iluminando” otras letras, ya que Matter.js no tiene detección de colisión direccional.
La solución fue calcular el ángulo entre la dirección de la linterna (usando el ángulo del cuerpo “T”) y el vector hacia cada letra,
combinando esa lógica con la distancia para determinar si una letra estaba iluminada. Este fue un reto tanto técnico como conceptual, y se resolvió con vectores y trigonometría simple.

#### Aprendizaje principal
El aprendizaje más significativo fue cómo combinar una simulación física con expresividad visual y narrativa.
Entendí que la física no solo sirve para realismo, sino también para comunicar ideas y significados de forma poética e interactiva.
Aprendí también que trabajar con motores físicos exige pensar más en dinámicas que en posiciones fijas,
lo cual cambia por completo la forma de diseñar una animación o experiencia visual.

