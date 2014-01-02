## Iteración y propiedades de un Array

A pesar de que los arrays, en JavaScript, sean objetos, no existen buenas razones para utilizar el bucle [`for in`](#object.forinloop) al momento de recorrerlos. De hecho, hay un buen número de razones en **contra** de la utilización de `for in` en arrays.

> **Nota:** Los arrays en JavaScript **no son** *arrays asociativos*. JavaScript sólo posee [objectos](#object.general) para mapear claves con valores. Y mientras que los arrays asociativos **preservan** el orden, los objetos no lo hacen.

Debido a que el bucle `for in` enumera todas las propiedades que existen en la cadena de prototipos y que la única forma de poder excluir esa propiedades es utilizando [`hasOwnProperty`](#object.hasownproperty), `for in` termina siendo hasta **veinte veces** más lento que un bucle normal `for`.

### Iteración

En orden de obtener el mejor desempeño al momento de iterar arrays, lo recomendable es utilizar el clásico bucle `for`.

    var list = [1, 2, 3, 4, 5, ...... 100000000];
    for(var i = 0, l = list.length; i < l; i++) {
        console.log(list[i]);
    }

En el ejemplo anterior se puede realizar una pequeña mejora: guardar la longitud del array utilizando `l = list.length`.

A pesar de que la propiedad `length` se encuentra definida dentro del mismo array, preguntar por su valor en cada iteración representa un costo adicional. Y aunque los motores de JavaScript modernos **puedan** aplicar optimizaciones para estos casos, no existe forma de saber cuando el código será ejecutado por alguno de ellos.

De hecho, no guardar la longitud del array en una variable puede resultar que el bucle sea la **mitad de rápido** que haciendolo.

### La propiedad `length`

Mientras que el *getter* de la propiedad `length` simplemente devuelve el número de elementos que contiene el array, el *setter* puede ser utilizado para **truncar** al elemento.

    var foo = [1, 2, 3, 4, 5, 6];
    foo.length = 3;
    foo; // [1, 2, 3]

    foo.length = 6;
    foo.push(4);
    foo; // [1, 2, 3, undefined, undefined, undefined, 4]

Asignando un valor menor, el array queda truncado, mientras que asignando uno mayor crea un array disperso.

### En Conclusión

Para obtener un mejor desempeño, es recomendable siempre utilizar el bucle clásico `for` y guardar en una variable el valor de la propiedad `length` al momento de iterar. La utilización de `for in` es un signo de código poco optimizado, propenso a errores.