#### ¿Qué resultado esperas obtener?
Esperamos que el vector inicialmente definido se modifique luego de llamar a la función playingVector(). 
Esto se evidenciará en la consola mediante los mensajes impresos (antes y después de la modificación) y, en el lienzo, se dibujará un círculo en la posición (20, 30).

#### ¿Qué resultado obtuviste?

#### Recuerda los conceptos de paso por valor y paso por referencia en programación. Muestra ejemplos de este concepto en javascript.
Por valor
```
function modificarPrimitivo(x) {
  x = 10;
}
let a = 5;
modificarPrimitivo(a);
console.log(a); // Imprime 5, ya que 'a' no cambió
```
Por referencia
```
function modificarObjeto(obj) {
  obj.valor = 10;
}
let b = { valor: 5 };
modificarObjeto(b);
console.log(b.valor); // Imprime 10, ya que 'b' fue modificado dentro de la función
```

#### ¿Qué tipo de paso se está realizando en el código?
En el código se está realizando un paso por referencia.
Cuando se pasa el vector position a la función playingVector(), se le está pasando una referencia al mismo objeto.
Por ello, al modificar v.x y v.y dentro de la función, se modifica el objeto original.

#### ¿Qué aprendiste?
Los objetos se pasan por referencia a las funciones.
Esto significa que cualquier modificación realizada en un objeto dentro de una función afectará al objeto original.
La utilidad de las funciones console.log() y print() para depurar el código y visualizar cómo se modifican los datos en tiempo de ejecución.
Este concepto es fundamental para entender el comportamiento de los datos al manipularlos dentro de funciones y para evitar efectos secundarios no deseados en programas más complejos.
