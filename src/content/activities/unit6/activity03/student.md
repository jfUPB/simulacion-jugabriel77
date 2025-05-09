#### Separación:

* Objetivo: Evitar que los agentes (boids) se amontonen demasiado.

* Lógica: Si un agente detecta que otro está muy cerca, se aleja de él aplicando una fuerza opuesta a la posición del vecino.

#### Alineación:

* Objetivo: Hacer que los agentes se muevan en la misma dirección que sus vecinos.

* Lógica: Cada agente calcula la dirección promedio de los agentes cercanos y ajusta su propia velocidad para alinearse con ese promedio.

#### Cohesión:

* Objetivo: Mantener a los agentes juntos como grupo, sin dispersarse demasiado.

* Lógica: Cada agente calcula el centro de masa de sus vecinos cercanos y se dirige ligeramente hacia él.

#### Parámetros clave identificados:

* Radio de percepción: Distancia dentro de la cual un agente considera a otros como sus vecinos para cada regla.

* Pesos de las reglas: Factores que controlan cuánto influye cada regla (separación, alineación y cohesión) en el comportamiento final del agente.

* maxspeed: Velocidad máxima que puede alcanzar un agente.

* maxforce: Máxima fuerza que un agente puede aplicar para cambiar su dirección o velocidad.

#### Modificacion:
Si en un Flocking los agentes o particulas involucrados no tienen un collider entre ellos, se empiezan a sobreponer, por ende, con el timpo siempre van a tender a unificar su movimiento y parecerá una sola particula flotando
