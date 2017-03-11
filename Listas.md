


----------


![enter image description here](https://lh3.googleusercontent.com/-vaN2_tM23W8/WMNSiu4l6aI/AAAAAAAAAA4/mKitVv9N6i4vfWTldwEy979rfUdPnyK_gCLcB/s0/pro.gif "pro.gif")


----------


**Listas de Prolog**
----------------

Una lista es, ya sea vacia o compuesta por un primer elemento cabeza y una cola, la cual es solo una lista por si misma. En prolog representamos la lista vacia por un atomo [] y una lista no vacia por un termino [H|T], donde H denota head cabeza y T denota tail cola


----------


**1.01 (*) Encuentra el último elemento de una lista.**
 	

> **Ejemplo:**
          

    ?- mi_ultima(X,[a,b,c,d]).
    X = d

**1.02 (*) Encuentra el ultimo, pero un elemento de una lista.** 
             (*de: zweitletztes Element, fr: avant-dernier élément*)




             
**1.03 (*) Encuentra el elemento K'th de una lista. El primer elemento en la lista es el número 1.**

> **Ejemplo:**
     

     ?- element_at(X,[a,b,c,d,e],3). 
     X = c





**1.04 (*) Encuentra el numero de elementos de una lista.**





**1.05 (*) Retroceder una lista.**





**1.06 (*) Averigüe si una lista es un palíndromo.**
Un palíndromo puede ser leído al derecho o al revés; e.g. [x,a,m,a,x].





**1.07 (**) Aplanando una estructura de una lista anidad.**
Transforme una lista, posiblemente manteniendo las listas como elementos en una lista 'plana' reemplazando cada lista con sus elementos (recursivamente).

> **Ejemplo:**

  

    ?- my_flatten([a, [b, [c, d], e]], X).
    X = [a, b, c, d, e]

Pista: Usar el predicado predefinido is_list/1 and append/3




**1.08 (**)Eliminar duplicados consecutivos de los elementos de una list.**

Si una lista contiene elementos repetidos, deberían ser reemplazados con una copia sencilla del elemnto. El orden de los elementos de no deben de ser cambiados.

> **Ejemplo:**

      

 ?- compress([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
 X = [a,b,c,a,d,e]

**1.09 (**)Si una lista contiene elementos repetidos, deberían ser colocados en sublistas separadas.**





> **Ejemplo:**

    ?- pack([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[a,a,a,a],[b],[c,c],[a,a],[d],[e,e,e,e]]





**1.10 (*)Codificación de longitud de ejecución de una lista.**

Utilice el resultado del problema 1.09 para implementar el denominado método de compresión de datos de codificación de longitud de ejecución. Los duplicados consecutivos de elementos se codifican como términos [N, E] donde N es el número de duplicados del elemento E.

> **Ejemplo:**

    ?- encode([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],[1,b],[2,c],[2,a],[1,d],[4,e]]





**1.11 (*)Codificación de longitud de ejecución modificada.**

Modifique el resultado del problema 1.10 de tal manera que si un elemento no tiene duplicados simplemente se copia en la lista de resultados. Sólo los elementos con duplicados se transfieren como términos [N, E].

> **Ejemplo:**

    ?- encode_modified([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],b,[2,c],[2,a],d,[4,e]]


----------


**1.12 ( * *)Decodificar una lista codificada de longitud de ejecución.**

Dada una lista de códigos de longitud de ejecución generada como se especifica en el problema 1.11. Construya su versión sin comprimir.


----------


**1.13 ( * *)Codificación de longitud de ejecución de una lista (solución directa).**

Implementar directamente el método de compresión de datos de codificación de longitud de ejecución. Es decir. No crear explícitamente las sublistas que contienen los duplicados, como en el problema 1.09, pero sólo contarlos. Como en el problema 1.11, simplifique la lista de resultados reemplazando los términos singleton [1, X] por X.

> **Ejemplo:**

    ?- decodificar_directo([a,a,a,a,b,c,c,a,a,d,e,e,e,e],X).
    X = [[4,a],b,[2,c],[2,a],d,[4,e]]


----------


**1.14 (*) Duplicar los elementos de una lista.**

> **Ejemplo:**

    ? - dupli ([a, b, c, c, d], X).
    X = [a, a, b, b, c, c, c, c, d, d]


----------


**1.15 ( * *) Duplicar los elementos de una lista un número dado de veces.**

> **Ejemplo:**

    \ Alpha - dupli ([a, b, c], 3, X).
    X = [a, a, a, b, b, b, c, c, c]
    ¿Cuáles son los resultados de la meta:
    \ Alpha - dupli (X, 3, Y).


----------


**1.16 ( * *)Soltar cada elemento N'th de una lista.**

> **Ejemplo:**

    ?- soltar([a,b,c,d,e,f,g,h,i,k],3,X).
    X = [a,b,d,e,g,h,k]


----------


**1.17 (*) Dividir una lista en dos partes; Se da la longitud de la primera parte.**
No utilice predicados predefinidos.

> **Ejemplo:**

    ?-, dividida ([a, b, c, d, e, f, g, h, i, k], 3, L1, L2).
    L1 = [a, b, c]
    L2 = [d, e, f, g, h, i, k]


----------


**1.18 ( * *) Extrae una tira de una lista.**
Dado dos índices, I y K, la tira es la lista que contiene los elementos entre el I'th y K'th elemento de la lista original (ambos límites incluidos). Comienza contando los elementos con 1.

> **Ejemplo:**

    ?- \ tira{(b), (b), a, b, c, d, e, f, g, h, i, k], 3,7, L).
    L = [c, d, e, f, g]


----------


**1.19 ( * *) Rotar una lista N hacia la izquierda.**

> **Ejemplo:**

    ?- rotar ([a, b, c, d, e, f, g, h], 3, X).
    X = [d, e, f, g, h, a, b, c]
    ?- rotar ([a, b, c, d, e, f, g, h], - 2, X).
    X = [g, h, a, b, c, d, e, f]

Pista: Utilice los predicados predefinidos length / 2 y append / 3, así como el resultado del problema 1.17.


----------


**1.20 (*) Quite el elemento K'th de una lista.**

> **Ejemplo:**

    ? - remove_at (X, [a, b, c, d], 2, R).
    X = b
    R = [a, c, d]).


----------


**1.21 (*) Insertar un elemento en una posición determinada en una lista.**

> **Ejemplo:**

    ? - insert_at (alfa, [a, b, c, d], 2, L).
    L = [a, alfa, b, c, d]


----------


**1.22 (*) Crea una lista que contenga todos los enteros dentro de un rango dado.**

> **Ejemplo:**

    ?- range(4,9,L).
    L = [4,5,6,7,8,9]


----------


**1.23 ( * *) Extrae un número dado de elementos seleccionados aleatoriamente de una lista.**
Los elementos seleccionados se incluirán en una lista de resultados.

> **Ejemplo:**

    ? - rnd_select ([a, b, c, d, e, f, g, h], 3, L).
    L = [e, d, a]

Pista: Utilice el generador de números aleatorios al azar / 2 y el resultado del problema 1.20.


----------


**1.24 (*) Lotería: Dibuja N números aleatorios diferentes del conjunto 1.M.**
Los números seleccionados se incluirán en una lista de resultados.

> **Ejemplo:**

    \ Alpha - lotto (6,49, L).
    L = [23,1,17,33,21,37]

Pista: Combinar las soluciones de los problemas 1.22 y 1.23.


----------


**1.25 (*) Genera una permutación aleatoria de los elementos de una lista.**

> **Ejemplo:**

    ? - rnd_permu ([a, b, c, d, e, f], L).
    L = [b, a, d, c, e, f]

Pista: Utilice la solución del problema 1.23.


----------


**1.26 ( * *) Generar las combinaciones de K objetos distintos elegidos de los N elementos de una lista.**
¿De cuántas maneras puede un comité de 3 ser elegido de un grupo de 12 personas? Todos sabemos que hay C (12,3) = 220 posibilidades (C (N, K) denota el bien conocido binomial coeficientes). Para los matemáticos puros, este resultado puede ser grande. Pero queremos realmente generar todas las posibilidades (a través de backtracking).

> **Ejemplo:**

    \ Alpha - combinación (3, [a, b, c, d, e, f], L).
    L = [a, b, c];
    L = [a, b, d];
    L = [a, b, e];
    ...


**1.27 ( * *) Agrupar los elementos de un conjunto en subconjuntos disjuntos.**

A) ¿De cuántas maneras puede un grupo de 9 personas trabajar en 3 subgrupos disjuntos de 2, 3 y 4 personas? Escriba un predicado que genere todas las posibilidades a través de retroceso.

> **Ejemplo:**

    ? - grupo3 ([aldo, beat, carla, david, evi, flip, gary, hugo, ida], G1, G2, G3).
    G1 = [aldo, beat], G2 = [carla, david, evi], G3 = [flip, gary, hugo, ida]
     ...

B) Generalice el predicado anterior de una manera que podemos especificar una lista de tamaños de grupo y el predicado devolverá una lista de grupos.

> **Ejemplo:**

    ? - grupo ([aldo, beat, carla, david, evi, flip, gary, hugo, ida], [2,2,5], Gs).
    Gs = [[aldo, beat], [carla, david], [evi, flip, gary, hugo, ida]]
     	...

Tome en cuenta que no queremos permutaciones de los miembros del grupo; Es decir, [[aldo, beat], ...] es la misma solución que [[beat, aldo], ...]. Sin embargo, hacemos una diferencia entre [[aldo, beat], [carla, david], ...] y [[carla, david], [aldo, beat], ...].

Puede encontrar más información sobre este problema combinatorio en un buen libro sobre matemáticas discretas bajo el término "coeficientes multinomiales".


----------


**1.28 ( * *) Clasificar una lista de listas según la longitud de las sublistas.**

A) Suponemos que una lista (InList) contiene elementos que son listas ellos mismos. El objetivo es clasificar los elementos de InList de acuerdo a su longitud. P.ej. Listas cortas primero, listas más largas más adelante o viceversa.

 	

> **Ejemplo:**

    ? - lsort ([a, b, c], [d, e], [f, g, h], [d, e], [i, j, k, l], [m, n], [ O]], L).
    [F, g, h], [i, j, k, l] ]

B) Una vez más, suponemos que una lista (InList) contiene elementos que son listas ellos mismos. Pero esta vez el objetivo es clasificar los elementos de InList de acuerdo a su frecuencia de longitud; Es decir, en el predeterminado, donde la clasificación se hace de forma ascendente, las listas con longitudes raras se colocan primero, otras con una longitud más recuente vienen más tarde.

> **Ejemplo:**

    ? - lfsort ([[a, b, c], [d, e], [f, g, h], [d, e], [i, j, k, l], [m, n], [ O]], L).
    [D, e], [d, e], [m, n] ]

Observe que en el ejemplo anterior, las dos primeras listas en el resultado L tienen longitud 4 y 1, ambas longitudes aparecen sólo una vez. La tercera y cuarta lista tiene longitud 3; Hay dos lista de esta longitud. Y finalmente, las últimas tres listas tienen longitud 2. Esta es la longitud más frecuente.




![enter image description here](https://lh3.googleusercontent.com/sM_kpVx3GhRCwAEsjBIhn5KDlEt92picokUNJTtpWre3r3kqDytUk2FrBpk7hE9uOzbQEPk=s0 "SWI.png")


----------
