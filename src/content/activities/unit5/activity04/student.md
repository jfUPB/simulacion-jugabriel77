#### ¿Cómo se gestiona la creación y la desaparición de las partículas y cómo se gestiona la memoria?

* Creación de partículas: Las partículas se crean en la función setup(), donde se genera una malla de partículas basadas en un espaciamiento fijo (definido por spacing). Se utiliza un bucle doble para recorrer las filas y columnas de la malla y crear objetos de la clase Particle en cada posición.

* Desaparición de partículas: Las partículas no desaparecen explícitamente en este código. Sin embargo, las explosiones, representadas por objetos de la clase Explosion, eliminan a las partículas afectadas por la onda expansiva mediante la interacción de fuerzas. La memoria de las partículas que no son afectadas por la explosión sigue activa durante la ejecución del programa.

* Gestión de memoria: La memoria se gestiona de manera sencilla. Las partículas no se eliminan explícitamente del arreglo particles, pero las explosiones se gestionan usando un bucle que las elimina cuando terminan (finished() devuelve true cuando el radio de la explosión supera el límite máximo).

#### ¿Cómo se aplica el marco de movimiento motion 101 en los sistemas de partículas que analizaste en la actividad anterior?

El marco de movimiento en motion 101 (que involucra la aceleración, velocidad y posición) se aplica principalmente a través de la física de las partículas. Las partículas tienen propiedades de pos (posición), vel (velocidad) y acc (aceleración). En el código:

* Posición: Se actualiza en la función update(), sumando la velocidad a la posición.

* Velocidad: Se actualiza sumando la aceleración a la velocidad y luego multiplicando por un factor de amortiguación (damping) para suavizar el movimiento.

* Aceleración: Se calcula por la fuerza de resorte (springStrength), que trata de devolver las partículas a su posición original.

El marco de movimiento se utiliza al actualizar la posición de las partículas con base en las fuerzas (como la gravedad y la atracción de la explosión) que afectan su movimiento.

#### ¿Cómo se aplican fuerzas externas a los sistemas de partículas que trabajaste en la unidad? ¿Qué fuerzas se aplicaron y cómo están modeladas?

Fuerzas externas:

* Gravedad: Se aplica una fuerza de gravedad entre la partícula activa y otras partículas en la función draw(), donde se calcula una dirección de atracción (gDir) y una magnitud de la fuerza que depende de la distancia entre las partículas.

* Explosión: Las explosiones generan una onda expansiva que afecta a las partículas cercanas. La fuerza de la explosión se calcula en la función applyForce() de la clase Explosion, donde se calcula la distancia entre la partícula y el centro de la explosión, y se aplica una fuerza de impulso decreciente en función de la distancia.

* Modelado de las fuerzas: Ambas fuerzas (gravedad y explosión) están modeladas usando vectores. La dirección de la fuerza se determina con p5.Vector.sub(), y la magnitud de la fuerza se ajusta dependiendo de la distancia, disminuyendo a medida que la distancia aumenta.

#### ¿Cómo se aplicaste los conceptos de herencia y polimorfismo en los sistemas de partículas que trabajaste en la unidad?

* Herencia: Aunque no se ve explícitamente en el código que la clase Particle y la clase Explosion compartan una relación de herencia, podemos observar que las clases comparten la idea de tener un estado de pos, vel y acc. Sería posible crear una clase base, por ejemplo, PhysicalObject, de la que Particle y Explosion heredarían métodos comunes como update(), display() y applyForce(), si quisiéramos estructurarlo de esa manera.

* Polimorfismo: El polimorfismo se podría aplicar si tuviéramos una estructura de clases más generalizada. Por ejemplo, si tuviéramos una clase base común, tanto las partículas como las explosiones podrían tener su propio comportamiento específico para métodos como update() y display(), pero podrían ser tratados de manera similar en un contexto más general (por ejemplo, usando un arreglo de objetos de tipo PhysicalObject y llamando a update() sin importar si el objeto es una Particle o una Explosion).
