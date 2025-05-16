#### ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?
El método mag() calcula la magnitud (o longitud) de un vector, es decir, la raíz cuadrada de la suma de los cuadrados de sus componentes.
mag(): Calcula la magnitud real del vector, lo que implica obtener la raíz cuadrada de la suma de los cuadrados de los componentes.
magSq(): Calcula la magnitud al cuadrado, es decir, la suma de los cuadrados de los componentes, sin calcular la raíz cuadrada.

#### ¿Para qué sirve el método normalize()?
El método normalize() ajusta el vector para que tenga una magnitud de 1 (lo convierte en un vector unitario), manteniendo su dirección original.
Esto es útil para trabajar únicamente con la dirección, sin importar la escala.

#### Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?
calcula el producto escalar de dos vectores, que indica qué tan alineados están entre sí o la proyección de uno sobre el otro.

#### El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?
Versión de instancia: Se invoca sobre un objeto vector específico y recibe otro vector como argumento para calcular el producto escalar entre ambos.
Versión estática: Se llama directamente desde la clase sin necesidad de tener un objeto previo, y se le pasan dos vectores como parámetros para calcular su producto escalar.

#### ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? 
El producto cruz de dos vectores genera un nuevo vector perpendicular a ambos.
La dirección del vector resultante se determina utilizando la regla de la mano derecha (lo que define su orientación), y su magnitud es igual al área del paralelogramo formado por los dos vectores,
es decir, es el producto de las magnitudes de los vectores multiplicado por el seno del ángulo entre ellos.

#### ¿Para que te puede servir el método dist()?
Se utiliza para calcular la distancia entre dos puntos (o vectores), lo que es útil para determinar la separación espacial entre ellos en un espacio dado.

#### ¿Para qué sirven los métodos normalize() y limit()?
normalize(): Sirve para convertir un vector en uno unitario (con magnitud 1), conservando su dirección.
limit(): Permite restringir la magnitud del vector a un valor máximo especificado, lo que es muy útil en simulaciones o cálculos físicos para evitar que ciertos valores (como velocidad o fuerza) excedan límites deseados.
