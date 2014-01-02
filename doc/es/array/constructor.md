## El Constructor `Array`

Debido a que el constructor `Array` es ambiguo en la manera con que trata sus parámetros, al momento de crear un nuevo array es altamente recomendable usar la notación literal `[]`:

    [1, 2, 3]; // Resultado: [1, 2, 3]
    new Array(1, 2, 3); // Resultado: [1, 2, 3]

    [3]; // Resultado: [3]
    new Array(3); // Resultado: []
    new Array('3') // Resultado: ['3']

En los casos que sólo se pasa un argumento al constructor `Array` y dicho es un valor del tipo `Number`, el constructor devolverá un nuevo array *disperso* con la propiedad `length` asignada con el valor del argumento. Notar que *únicamente* la propiedad `length` del nuevo array será seteada; los indices del array no serán inicializados.

    var arr = new Array(3);
    arr[1]; // undefined
    1 in arr; // false, el índice no fue asignado
    
Manipular la propiedad length de un array es sólo útil en unos pocos casos, como por ejemplo en la repetición de un string, permitiendo en ese caso evitar la utilización de un bucle.

    new Array(count + 1).join(stringToRepeat);

### En Conclusión

La notación literal es preferible a la utilización del constructor del Array. Dicha notación tiene como beneficios ser más corto, tener una sintaxis más clara y ayuda a mejorar la lectura del código.