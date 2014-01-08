## El Operador `delete`

Siendo breves, en JavaScript es *imposible* eliminar variables globales, funciones y otros elementos que posean el atributo `DontDelete` activado.

### Contexto global y contexto de una función

Cuando una variable o una función es definida en el contexto global o en el [contexto de una función](#function.scopes) es responsabilidad de ambas dos el Objeto de Activación o el Objeto Global (respectivamente). Esas propiedades poseen un grupo de atributos, en el cual uno es `DontDelete`. 

Las variables y funciones declaradas de manera global o dentro de funciones siempre crean propiedades con `DontDelete` activado y por lo tanto, no pueden ser eliminadas. 

    // variable global:
    var a = 1; // DontDelete esta activado
    delete a; // false
    a; // 1

    // función normal:
    function f() {} // DontDelete esta activado
    delete f; // false
    typeof f; // "function"

    // reasignando no ayuda:
    f = 1;
    delete f; // false
    f; // 1

### Propiedades explicitas

Las propiedades creadas explícitamente pueden ser eliminadas normalmente.

    // propiedad creada explicitamente:
    var obj = {x: 1};
    obj.y = 2;
    delete obj.x; // true
    delete obj.y; // true
    obj.x; // undefined
    obj.y; // undefined

En el ejemplo anterior, `obj.x` y `obj.y` pueden ser eliminados porque no poseen el atributo `DontDelete`. Y por eso mismo el siguiente ejemplo también funciona:

    // lo siguiente funciona bien, exceptuando IE:
    var GLOBAL_OBJECT = this;
    GLOBAL_OBJECT.a = 1;
    a === GLOBAL_OBJECT.a; // true - tan solo una variable global
    delete GLOBAL_OBJECT.a; // true
    GLOBAL_OBJECT.a; // undefined

Aquí se utiliza un truco para eliminar `a`. En el ejemplo, [`this`](#function.this) hace referencia al Objeto Global. Explícitamente se declara la variable `a` como su propiedad, permitiendo eliminarla.

En IE (al menos en las versiones 6-8) el código anterior no funciona correctamente,

### Argumentos y propiedades internas de funciones

Los argumentos, los objetos [`arguments`](#function.arguments) y las propiedades internas de una función también poseen el atributo `DontDelete` activado.

    // argumentos y propiedades de una función
    (function (x) {
    
      delete arguments; // false
      typeof arguments; // "object"
      
      delete x; // false
      x; // 1
      
      function f(){}
      delete f.length; // false
      typeof f.length; // "number"
      
    })(1);

### Objetos huéspedes

El funcionamiento del operador `delete` puede ser impredecible al utilizarlos en objetos huéspedes. Debido a su especificación, este tipo de objetos tienen permitido implementar cualquier tipo de comportamiento.

### En conclusión

El operador `delete` a menudo posee comportamientos inesperados y únicamente puede ser utilizado sin peligro eliminando propiedades creadas de manera explícita en objetos normales.
