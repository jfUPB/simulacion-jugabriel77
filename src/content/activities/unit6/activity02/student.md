#### 1. Estructura de datos del campo de flujo:

* El campo de flujo se representa como una rejilla (grilla) de celdas.

* Cada celda almacena un vector de dirección.

* Internamente, suele implementarse como un array (arreglo) de vectores.

#### 2. Cómo se generan los vectores:

Los vectores en cada celda se generan generalmente de manera aleatoria o usando ruido Perlin para que las direcciones cambien suavemente en el espacio, imitando patrones naturales de movimiento como el viento o el agua.

#### 3. Cómo un agente usa el campo para calcular su fuerza de dirección:

* El agente localiza la celda del campo de flujo donde se encuentra (según su posición).

* Lee el vector almacenado en esa celda.

* Usa ese vector como una fuerza de dirección, ajustándolo si es necesario (por ejemplo, aplicando fuerzas límite o escalas de velocidad).

#### 4. Parámetros clave identificados:

* Resolución: Tamaño de cada celda de la rejilla (afecta la "fineza" del campo).

* maxspeed: Velocidad máxima que puede alcanzar el agente.

* maxforce: Fuerza máxima que puede aplicar el agente para cambiar su dirección.

#### Modificaciones
La modificacion que realice fue agregar dos polos magneticos simulando los de la tierra para ver como cambiaba al flujo, este forma una especie de figura simulando el numero ocho o un infinito pero siempre beneficia un lado, nunca es simetrico.
