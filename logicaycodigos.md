
**3.Logica y Codigos**
-------------
Las soluciones las puedes encontrar aquí

----------




**3.01 (* *) Tablas de verdades para expresiones lógicas.**

Definir predicados y / 2, o / 2, nand / 2, nor / 2, xor / 2, impl / 2 y equ / 2 (para equivalencia lógica) que tienen éxito o fallan según el resultado de sus operaciones respectivas; p.ej. Y (A, B) tendrán éxito, si y sólo si ambos A y B tienen éxito. Tenga en cuenta que A y B pueden ser objetivos de Prolog (no sólo las constantes verdaderas y fallas).
Una expresión lógica en dos variables se puede escribir en la notación de prefijo, como en el siguiente ejemplo: y (o (A, B), nand (A, B)).

Ahora, escriba una tabla de predicados / 3 que imprima la tabla de verdad de una expresión lógica dada en dos variables.

**Ejemplo:**

    ?- table(A,B,and(A,or(A,B))).
| true | true | true |
|:-------------------|:--------:|-------------------:|
| true | fail | true |
| fail | true | fail |
| fail | fail | fail |


**3.02 (*) Tablas de verdades para expresiones lógicas (2).**

Continúe el problema 3.01 definiendo y / 2, o / 2, etc como operadores. Esto permite escribir la expresión lógica de la manera más natural, como en el ejemplo: A y (A o no B). Defina la precedencia del operador como de costumbre; Como en Java.

**Ejemplo:**

    ?- table(A,B, A and (A or not B)).
    
| true | true | true |
|:-------------------|:--------:|-------------------:|
| true | fail | true |
| fail | true | fail |
| fail | fail | fail |


**3.03 (* *) Tablas de verdades para expresiones lógicas (3).**

Generalizar problema 3.02 de tal manera que la expresión lógica puede contener cualquier número de variables lógicas. Definir tabla / 2 de una manera que tabla (List, Expr) imprime la tabla de verdad para la expresión Expr, que contiene las variables lógicas enumeradas en List.

**Ejemplo:**

    ?- table([A,B,C], A and (B or C) equ A and B or A and C).

true | true | true |true
--- | --- | --- | ---
true | true | fail |true
true | fail | true |true
true | fail | fail |true
fail | true | true |true
fail | fail | true |true
fail | fail | fail |true






**3.04 (* *) Código gris.**

Un código Gray de n bits es una secuencia de cadenas de n bits construidas de acuerdo con ciertas reglas. Por ejemplo,

 - N = 1: C (1) = ['0', '1'].
 - N = 2: C (2) = ['00', '01', '11', '10'].
 - N = 3: C (3) = ['000', '001', '011', '010', '110', '111', '101',
   '100'].

Investigue las reglas de construcción y escriba un predicado con la siguiente especificación:
% Gris (N, C): - C es el código N-bit Gray
¿Se puede aplicar el método de "caché de resultados" para hacer que el predicado sea más eficiente, cuando se va a usar de forma repetida?

**3.05 (* * *) código de Huffman.**

En primer lugar, estudiar un buen libro sobre matemáticas discretas o algoritmos para una descripción detallada de los códigos Huffman, o consultar Wikipedia

Suponemos un conjunto de símbolos con sus frecuencias, dado como una lista de términos fr (S, F). Ejemplo: [fr (a, 45), fr (b, 13), fr (c, 12), fr (d, 16), fr (e, 9), fr (f, 5)]. Nuestro objetivo es construir una lista hc (S, C) términos, donde C es la palabra de código Huffman para el símbolo S. En nuestro ejemplo, el resultado podría ser Hs = [hc (a, '0'), hc (b , '101'), hc (c, '100'), hc (d, '111'), hc (e, '1101' ), ... etc.]. La tarea será realizada por el predicado huffman / 2 definido como se presenta:

% Huffman (Fs, Hs): - Hs es la tabla de códigos de Huffman para la tabla de frecuencias Fs.


----------



_ _ _ _ _




