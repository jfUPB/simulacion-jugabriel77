#### Paso por valor
significa que se crea una copia del valor original cuando se pasa a una función o se asigna a una variable. Cualquier cambio que se haga sobre esa copia no afectará al valor original.

#### Paso por referencia 
significa que se pasa la referencia (o dirección) al valor original. Cualquier cambio que se haga sobre la variable referenciada afectará directamente al valor original.

 En el segundo caso ```(.copy())```, ```friction``` es independiente de ```this.velocity```, lo que te permite manipular ```friction``` sin alterar el valor de ```this.velocity```. Sin ```.copy()```, 
 cualquier cambio en ```friction``` afectará a ```this.velocity``` también.
