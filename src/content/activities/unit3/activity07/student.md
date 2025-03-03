 #### ¿Qué problema le ves a este planteamiento y qué solución propones. ¿Cómo lo implementarías en p5.js?
En JavaScript, los objetos (como p5.Vector) se pasan por referencia, lo que significa que cualquier cambio realizado dentro de la función sobre force también afectará a la variable original
```
force.div(10);
this.acceleration.add(force);
```
modificar la variable force directamente, dividiéndola por 10. 
Esto cambia permanentemente el vector force que fue pasado a la función, lo que puede no ser el comportamiento esperado, ya que puede afectar a otras partes del código que dependen de estos vectores.

Para evitar este problema, la solución es crear una copia del vector force antes de modificarlo.
De esta manera, no se modifica el vector original, sino que se trabaja con una copia local, evitando efectos secundarios no deseados.
```
applyForce(force) {
    // Crear una copia del vector force para evitar modificar el original
    let f = force.copy();
    
    // Asumir que la masa es 10
    f.div(10);
    this.acceleration.add(f);
}
```
```force.copy():``` Este método crea una nueva instancia del objeto ```p5.Vector```, que es una copia exacta del vector ```force```. 
Ahora podemos modificar esta copia sin que afecte al vector original.
```f.div(10):``` Ahora la operación de división se realiza sobre la copia del vector, manteniendo intacto el vector original.
```this.acceleration.add(f):``` Se añade la copia del vector modificado a la aceleración, sin alterar la variable ```force``` que fue pasada a la función.
