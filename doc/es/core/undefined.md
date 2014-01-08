## `undefined` y `null`

JavaScript posee dos valores distintos para determinar cuando algo no existe/es vacío: `null` y `undefined`. Sin embargo éste último el más útil para la mayoría de los casos.

### El Valor `undefined`

`undefined` es un *tipo* que posee un solo valor: `undefined`. 

El lenguaje además define una variable global que posee el valor `undefined`; ésta variable es justamente también llamada `undefined`. Sin embargo dicha variable **no es** una constante ni una palabra reservada. Por lo tanto su *valor* puede ser fácilmente sobre-escrito.

> **Nota sobre ES5:** En ECMAScript 5 (y en modo estricto), `undefined` **no puede** ser 
> *sobre-escrito*. Sin embargo puede ser reemplazado, por ejemplo, por una función con 
> el nombre `undefined`

A continuación se detallan algunos ejemplos de cuando el valor `undefined` es devuelto:

 - Al acceder a la variable global `undefined` (sin modificar).
 - Accediendo a una variable declarada *pero aún* no inicializada.
 - Declaraciones `return` que explícitamente no devuelven nada.
 - Al buscar propiedades que no existen.
 - Parámetros de funciones que no poseen ningún valor explícito pasado.
 - Cualquier cosa que haya sido definida con el valor de `undefined`
 - Cualquier expresión con el formato `void(expression)`

### Manejando los Cambios en el Valor de `undefined`

Debido a que la variable global `undefined` sólo guarda una copia del *valor* actual de `undefined`, asignar un nuevo valor a ella **no** modifica el valor del *tipo* `undefined`.

Por lo tanto, para poder comparar algo contra el valor de `undefined`, es necesario obtener primero el valor original de `undefined`.

Para ello, una técnica común es añadir un parámetro adicional en una [función anónima](#function.scopes) a la cual no se le pasa ningún valor.

    var undefined = 123;
    (function(something, foo, undefined) {
        // en el contexto local, undefined
        // refiere al valor `undefined`

    })('Hello World', 42);

Otra manera de obtener el mismo efecto es escribiendo una declaración dentro de la función.

    var undefined = 123;
    (function(something, foo) {
        var undefined;
        ...

    })('Hello World', 42);

La única diferencia aquí es que esta versión utiliza 4 bytes más en caso ser minimizado el código y no haya otra declaración `var` dentro de la función.

### Utilización de `null`

Mientras que, en el contexto de JavaScript, `undefined` es utilizado mayormente en el sentido del tradicional *null*, el actual `null` (en su forma literal y de *tipo*) es tan solo otro tipo de dato.

El mismo es utilizado en algunos casos (por ejemplo, al declarar el final de una cadena de prototipos: `Foo.prototype = null`), pero en la mayoría de los casos puede ser reemplazado por `undefined`.