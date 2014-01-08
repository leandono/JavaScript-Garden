## Inserción Automática de Punto y Coma

A pesar que JavaScript posea un estilo de sintaxis similar a C, el mismo **no impone** la obligación de utilizar los punto y coma en el código fuente, de manera que pueden ser omitidos.

Sin embargo, JavaScript no es un lenguaje en donde no se utilizan los punto y coma. De hecho, el parseador de JavaScript los necesita para entender el código fuente. Y en caso de existir un error debido a un punto y coma ausente, el parseador lo insertará de manera **automática**.

    var foo = function() {
    } //error de parseo, se espera un punto y coma
    test()

Se ejecuta la inserción del punto y coma, el parseador intenta ejecutar el código nuevamente.

    var foo = function() {
    }; // sin error, el parseador continúa
    test()

La inserción automática de los punto y coma es considerado como uno de las mayores fallas de diseño en el lenguaje debido a que el mismo **puede** cambiar el comportamiento del código.

## Funcionamiento

El código a continuación no posee ningún punto y coma, por lo tanto, deja la responsabilidad al parseador en decidir donde insertarlos.

    (function(window, undefined) {
        function test(options) {
            log('testing!')

            (options.list || []).forEach(function(i) {

            })

            options.value.test(
                'long string to pass here',
                'and another long string to pass'
            )

            return
            {
                foo: function() {}
            }
        }
        window.test = test

    })(window)

    (function(window) {
        window.someLibrary = {}

    })(window)

A continuación se muestra el resultado de cómo el parseador analizó e insertó los punto y coma.

    (function(window, undefined) {
        function test(options) {

            // No se inserta, la linea es unida
            log('testing!')(options.list || []).forEach(function(i) {

            }); // <- insertado

            options.value.test(
                'long string to pass here',
                'and another long string to pass'
            ); // <- insertado

            return; // <- insertado, interrumpe el return declarado
            { // comienzo de un nuevo bloque

                // una propiedad y expresión simple declarada
                foo: function() {} 
            }; // <- insertado
        }
        window.test = test; // <- insertado

    // La linea es unida nuevamente
    })(window)(function(window) {
        window.someLibrary = {}; // <- insertado

    })(window); //<- insertado


> **Nota:** El parseador de JavaScript no maneja de manera "correcta" las declaraciones 
> return que son seguidas por una nueva línea. Si bien no es necesariamente un defecto de 
> la inserción automática de punto y coma, el mismo puede terminar aplicando un efecto no deseado.

Como se muestra, el parseador modifica de manera drástica el comportamiento del código. Y en ciertos casos, lo hace de **manera errónea**.



### Paréntesis

En el caso de los paréntesis, el parseador **no** insertará un punto y coma en el código.

    log('testing!')
    (options.list || []).forEach(function(i) {})

Por lo tanto, el código quedará en una sola línea.

    log('testing!')(options.list || []).forEach(function(i) {})

Es **muy** probable que `log` **no** devuelva una función; por lo tanto el código anterior retornará un `TypeError` describiendo que `undefined is not a function`.

### En Conclusión

Es muy recomendable **nunca** omitir los punto y coma. Además es recomendable dejar los llaves en la misma línea que su declaración correspondiente y nunca omitirlos para las declaraciones `if` / `else` escritas en una sola linea. Estas medidas no solo mejorarán la consistencia del código sino que también prevendrán que el parseador JavaScript cambie el comportamiento del mismo.
