#### Diferencias fundamentales
* Flow Fields: La inteligencia está en el entorno. El campo de flujo (una cuadrícula de vectores) guía el movimiento de los agentes de manera pasiva: las partículas simplemente siguen las direcciones que el entorno les da.

* Flocking: La inteligencia está en las interacciones entre los agentes. Cada agente toma decisiones basadas en los demás (separación, alineación y cohesión), no en un entorno externo predefinido.


#### Tipos de comportamiento emergente
* Flow Fields: genera patrones suaves y continuos como corrientes, remolinos o flujos orgánicos. Ejemplo: simulación de humo o agua.

* Flocking: produce agrupamientos dinámicos, bandadas, enjambres o rebaños. Ejemplo: un grupo de pájaros o peces moviéndose juntos.


#### Ventajas y desventajas
##### Flow Fields:

* Ventajas: Muy bueno para visualizaciones fluidas, paisajes dinámicos y efectos atmosféricos. Es simple de controlar.

* Desventajas: No hay interacción social entre partículas. Puede resultar menos "vivo" o "inteligente".

##### Flocking:

* Ventajas: Excelente para simular comportamientos colectivos naturales. Produce gran variedad de movimientos realistas e impredecibles.

* Desventajas: Requiere más cálculos (buscar vecinos, aplicar reglas), es más costoso computacionalmente.

#### El agente autónomo

* Tiene propiedades individuales (posición, velocidad).

* Toma decisiones locales basadas en su entorno o vecinos.

* No sigue un "guión" fijo, sino que responde dinámicamente.

En Flow Fields, el agente decide hacia dónde moverse basándose en el entorno.
En Flocking, el agente decide basándose en otros agentes.


#### Comportamiento emergente:

* En Flow Fields, cuando partículas independientes formaban espirales, ríos o torbellinos sin haberlos programado directamente.

* En Flocking, cuando los agentes se agrupaban, evitaban choques y cambiaban de dirección como un todo, a pesar de solo seguir reglas locales simples.
