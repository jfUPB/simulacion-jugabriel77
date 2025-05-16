#### 1. ¿Qué hace la función ```heading()```?
La función ```heading()``` se usa para obtener el ángulo de orientación de un vector en radianes. 
Es útil cuando trabajas con movimientos basados en vectores, ya que te permite saber en qué dirección está apuntando un objeto.

#### 2. ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.
Las funciones ```push()``` y ```pop()``` se utilizan para guardar y restaurar el estado de transformación del lienzo en p5.js.
Esto es útil cuando aplicas transformaciones como ```translate()```, ```rotate()```, y ```scale()```, y quieres asegurarte de que estas no afecten otros elementos del dibujo.

#### 3. ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.
La función rectMode(CENTER) cambia el modo en que se dibujan los rectángulos en p5.js.
Por defecto, rect(x, y, w, h) dibuja un rectángulo con la esquina superior izquierda en (x, y).
Cuando usas rectMode(CENTER), el punto (x, y) se convierte en el centro del rectángulo.

#### 4. ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad?
Cuando rotamos un objeto en p5.js, estamos cambiando la dirección en la que se mueve o apunta.
La rotación ocurre alrededor del origen de coordenadas actual,
lo que significa que si antes un objeto se movía en la dirección (1,0),
después de rotarlo 90° (π/2 radianes), se moverá en la dirección (0,1).
