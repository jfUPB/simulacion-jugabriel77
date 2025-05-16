#### Conceptos:

* Agente autónomo: El concepto fue claro; cada partícula se comporta como un agente que se mueve de forma independiente siguiendo ciertas reglas de movimiento (como el ruido Perlin y la separación).

* Emergencia: Fue relativamente entendible al ver cómo de reglas simples (movimiento + separación) surgía un patrón más complejo (una especie de red orgánica de conexiones).

* Flow fields: Entendí bien la idea gracias a cómo el ruido Perlin define una dirección de movimiento en cada punto del espacio, como si fuera un "campo de fuerzas".

* Flocking: Fue un poco más desafiante porque, aunque hay separación y algo de cohesión (al conectar con vecinos cercanos), no se implementa completamente el modelo de Reynolds (alineación, cohesión y separación completas).

Lo más fácil: Entender cómo el ruido Perlin afecta el movimiento.
Lo más difícil: La separación y el manejo de la cuadrícula espacial para optimizar las búsquedas de vecinos.

#### Análisis de algoritmos

Sí, experimentar cambiando parámetros (como separationDistance, noiseScale, o la speedFactor) ayudó mucho a entender cómo los cambios locales afectan el comportamiento global del sistema.

 ##### Dificultades:

 *Ajustar bien la escala del ruido (noiseScale) para que el movimiento fuera interesante y no caótico.

* Entender cómo organizar las partículas en una cuadrícula para encontrar vecinos cercanos eficientemente (sin la cuadrícula sería muy lento).

Aplicación creativa

* Me sentí relativamente exitoso aplicando el "Levy Flight" (salto aleatorio no gaussiano) con la tecla Espacio, porque le da un comportamiento errático e interesante a las partículas, como si de repente exploraran mucho más.

* Lo más desafiante: Lograr que el "Levy Flight" no rompiera completamente la coherencia del movimiento general o no sacara a demasiadas partículas de la pantalla.

#### Relación con unidades anteriores:

Ya habíamos trabajado antes con vectores, fuerzas y movimiento. Aquí todo eso se combina: fuerzas (sesgo, separación) aplicadas a agentes que se mueven siguiendo un sistema de reglas locales (similar a otros ejercicios de TNoC).

Este ejemplo lleva todo eso a un nivel más emergente: patrones que no programamos explícitamente sino que surgen del comportamiento combinado de las partículas.

#### Exploración futura
Aspectos que me interesan explorar más:

* Implementar flocking completo (añadiendo reglas de cohesión y alineación, como el modelo de Craig Reynolds).

* Explorar flow fields dinámicos más complejos, donde el campo de fuerza se modifique por la interacción de las propias partículas.

* Investigar simulaciones biológicas usando estos principios, como enjambres o migraciones.
* Ampliar mi entendeminiento en los modelos de reynolds
