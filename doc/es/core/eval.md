## Porqué no utilizar `eval`

La función `eval` ejecuta un string de código JavaScript en el contexto local.

    var foo = 1;
    function test() {
        var foo = 2;
        eval('foo = 3');
        return foo;
    }
    test(); // 3
    foo; // 1

Sin embargo, el código pasado a `eval` sólo se ejecutará en el contexto local cuando la función es llamada **directamente** *y* el nombre de la función llamada es `eval`.

    var foo = 1;
    function test() {
        var foo = 2;
        var bar = eval;
        bar('foo = 3');
        return foo;
    }
    test(); // 2
    foo; // 3

El uso de `eval` debe evitarse **a toda costa**. El 99.9% de los casos en que se "utiliza" pueden ser re-escritos **sin su utilización**.
    
### `eval` disfrazado

Las funciones de [tiempo de espera](#other.timeouts) `setTimeout` y `setInterval` también permiten ejecutar un string de código JavaScript cuando se pasa como primer argumento. En estos casos, el string se ejecutará **siempre** en el contexto global ya que `eval` no se llama de manera directa.

### Problemas de seguridad

La utilización de `eval` puede implicar problemas de seguridad ya que al ejecutar **cualquier** código enviado pueden ocurrir casos en que se ejecutan strings que no se conocen o poseen un origen no confiable.

### En conclusión

`eval` nunca debe ser usado, cualquier código que haga uso del mismo debe ser cuestionado
en su funcionamiento, rendimiento y seguridad. En caso que se necesite `eval` para que un código funcione, **no debe utilizar** en primer lugar. El código deberá repensarse de manera tal que no requiera la utilización de `eval`.
