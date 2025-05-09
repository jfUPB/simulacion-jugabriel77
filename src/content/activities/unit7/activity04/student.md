
#### Flujo de trabajo
El flujo de trabajo para integrar Matter.js con p5.js implicó inicializar el motor de física (Engine.create()), crear cuerpos físicos para cada letra y agregarlos al mundo en setup().
En draw(), se actualiza el motor físico con Engine.update(engine) y luego se dibujan los cuerpos (letras) usando sus propiedades físicas (posición y ángulo).
Además, se maneja la lógica de iluminación y movimiento del cuerpo "T", que actúa como linterna.


#### Representación visual vs. simulación física
Cada letra del texto se representa mediante un cuerpo físico (Bodies.circle) en Matter.js, y su visualización en p5.js se logra mediante text() posicionado con translate(b.position.x, b.position.y)
y rotado con rotate(b.angle). El principal reto fue sincronizar correctamente la rotación y posición de los cuerpos para que el texto se vea natural y se alinee con el comportamiento físico. 
También fue necesario manejar la iluminación condicional con base en el ángulo y distancia relativa respecto a la letra "T".


#### Creación de formas complejas
Para las letras, se usaron cuerpos circulares simples con una etiqueta (label) que indica qué letra representan. No se construyeron formas complejas con vértices o polígonos personalizados.
Esto facilitó la implementación, aunque limitó la fidelidad visual (las letras no colisionan exactamente con su forma visible). 
Usar Bodies.fromVertices() hubiera permitido mayor precisión, pero también habría aumentado la complejidad del código.


#### Física para la semántica
La simulación física ayuda a reforzar el significado de la palabra "LINTERNA", ya que la letra "T" actúa literalmente como una linterna que ilumina las demás letras al tocarlas o apuntarlas. 
Es un ejemplo efectivo de cómo una metáfora visual puede volverse interactiva gracias a la física. Este enfoque funciona mejor con palabras que representen acciones, objetos o conceptos físicos.
Sería más difícil aplicar esta técnica a palabras abstractas como "justicia" o "nostalgia", donde la representación visual no es tan directa.


#### Potencial exploratorio
Combinar p5.js y Matter.js abre muchas posibilidades creativas: desde tipografías interactivas y poesía animada hasta visualizaciones educativas (como fuerzas o gravedad). 
También se pueden explorar instalaciones artísticas digitales donde el texto reacciona a los movimientos del usuario o a sonidos, creando experiencias inmersivas o generativas.
