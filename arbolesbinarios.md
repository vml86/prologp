
**3.Arboles Binarios**
-------------
Las soluciones las puedes encontrar [aquí](https://github.com/vml86/prologp/blob/master/SolucionesAB.md)

----------

**Un árbol binario está vacío o está compuesto de un elemento raíz y dos sucesores, que son árboles binarios ellos mismos.**

![enter image description here](https://lh3.googleusercontent.com/-8OYlyx1_1nk/WMc7mpvmCdI/AAAAAAAAABM/uY8293EeYiIec3BZUA0zoQrMYXgz0aieQCLcB/s0/arbol.gif "arbol.gif")


En Prolog representamos el árbol vacío por el átomo 'nil' y el árbol no-vacío por el término t (X, L, R), donde X denota el nodo raíz y L y R denotan el subárbol izquierdo y derecho, respectivamente. Por lo tanto, el árbol de ejemplo representado de manera opuesta está representado por el siguiente término de Prolog:

T (t, t), t (t, t (t, t), t (t, n) Unesdoc.unesco.org unesdoc.unesco.org
Otros ejemplos son un árbol binario que consiste en un nodo raíz solamente:
T2 = t (a, nil, nil) o un árbol binario vacío: T3 = nil

**4.01 (*) Compruebe si un término dado representa un árbol binario**

Escribir un predicado istree / 1 que tiene éxito si y sólo si su argumento es un término Prolog que representa un árbol binario.

> **Ejemplo:**

 

     - ? - istree (t (a, t (b, nil, nil), nil)).
           Sí
        
     - ? - istree (t (a, t (b, nil, nil))).
              No

**4.02 ( * * ) Construir árboles binarios completamente equilibrados**

En un árbol binario completamente equilibrado, la siguiente propiedad es válida para cada nodo: El número de nodos en su subárbol izquierdo y el número de nodos en su subárbol derecho son casi iguales, lo que significa que su diferencia no es mayor que uno.

Escribe un predicado cbal_tree / 2 para construir árboles binarios completamente equilibrados para un número dado de nodos. El predicado debe generar todas las soluciones mediante backtracking. Ponga la letra 'x' como información en todos los nodos del árbol.

> **Ejemplo:**

    \ Alpha - cbal_tree (4, T).
    T = t (x, t (x, nil, nil), t (x, nil, t (x, nil, nil)));
    T = t (x, t (x, nil, nil), t (x, t (x, nil, nil), nil));

Etc.... No

**4.03 ( * * ) Árboles binarios simétricos**

Permitanos llamar a un árbol binario simétrico si puedes dibujar una línea vertical a través del nodo raíz y luego el subárbol derecho es la imagen especular del subárbol izquierdo. Escriba un predicado simétrico / 1 para comprobar si un árbol binario dado es simétrico. Sugerencia: Escriba un predicado espejo / 2 primero para comprobar si un árbol es la imagen especular de otro. Sólo estamos interesados en la estructura, no en el contenido de los nodos.

**4.04 ( * * ) Árboles binarios de búsqueda (diccionarios)**

Utilice el predicado add / 3, desarrollado en el capítulo 4 del curso, para escribir un predicado para construir un árbol de búsqueda binario a partir de una lista de números enteros.

> **Ejemplo:**

    \ Alpha - constructo ([3,2,5,7,1], T).
    T = t (3, t (2, t (1, nil, nil), nil), t (5, nil, t (7, nil, nil)
    A continuación, utilice este predicado para probar la solución del problema P56.

> **Ejemplo:**

    ? - test_symmetric ([5,3,18,1,4,12,21]).
    Sí
    ? - test_symmetric ([3,2,5,7,4]).
    No

**4.05 ( * * ) Generar y probar el paradigma**

Aplicar el paradigma de generar y probar para construir todos los árboles binarios simétricos, completamente equilibrados con un número dado de nodos. 

> **Ejemplo:**

    ? - sym_cbal_trees (5, Ts).
    T (x, t (x, nil, t (x, nil, nil)), t (x, t (x, nil, nil) X, nil, nil), t (x, nil, t (x, nil, nil)

¿Cuántos árboles existen con 57 nodos? Investigue cuántas soluciones hay para un número determinado de nodos? ¿Qué pasa si el número es par? Escribe un predicado apropiado.

**4.06 ( * * ) Construir árboles binarios de altura equilibrada**

En un árbol binario de altura equilibrada, la siguiente propiedad es válida para cada nodo: La altura de su subárbol izquierdo y la altura de su subárbol derecho son casi iguales, lo que significa que su diferencia no es mayor que uno.

Escribe un predicado hbal_tree / 2 para construir árboles binarios con altura equilibrada para una altura determinada. El predicado debe generar todas las soluciones mediante backtracking. Ponga la letra 'x' como información en todos los nodos del árbol.

> **Ejemplo:**

    \ Alpha - hbal_tree (3, T).
    (X, nil, nil), t (x, t (x, t (x, nil, nil) ));
    T = t (x, t (x, t (x, nil, nil), t (x, nil, nil)), t (x, t (x, nil, nil)

Etc...No

**4.07 ( * * ) Construir árboles binarios con equilibrio de altura con un número dado de nodos**

Considere un árbol binario con altura equilibrada de altura H. ¿Cuál es el número máximo de nodos que puede contener?
Claramente, MaxN = 2 ** H - 1. Sin embargo, ¿cuál es el número mínimo MinN? Esta pregunta es más difícil. Trate de encontrar una instrucción recursiva y convertirlo en un predicado minNodes / 2 definido de la siguiente manera:

% minNodos (H, N): - N es el número mínimo de nodos en un árbol binario con altura equilibrada de altura H. (Entero, entero), (+,?)

Por otro lado, podríamos preguntarnos: ¿cuál es la altura máxima H un árbol binario con equilibrio de altura con N nodos puede tener?

% maxHeight (N, H): - H es la altura máxima de un árbol binario con equilibrio de altura con N nodos
(Entero, entero), (+,?)
Ahora, podemos atacar el problema principal: construir todos los árboles binarios con equilibrio de altura con un número determinado de nodos.

% Hbal_tree_nodes (N, T): - T es un árbol binario equilibrado en altura con N nodos.
Investigue cuántos árboles con altura balanceada existen para N = 15.

**4.08 (*) Cuente las hojas de un árbol binario**

Una hoja es un nodo sin sucesores. Escribe un predicado count_leaves / 2 para contarlos.

% Count_leaves (T, N): - el árbol binario T tiene N hojas

**4.09 (*) Colecciona las hojas de un árbol binario en una lista**

Una hoja es un nodo sin sucesores. Escribe un predicado leaves / 2 para recogerlos en una lista.
% Hojas (T, S): - S es la lista de todas las hojas del árbol binario T

**4.10 (*) Recopilar los nodos internos de un árbol binario en una lista**

Un nodo interno de un árbol binario tiene uno o dos sucesores no vacíos. Escribe un predicado internals / 2 para recogerlos en una lista.

% Internals (T, S): - S es la lista de nodos internos del árbol binario T.

**4.11 (*) Recopilar los nodos en un nivel dado en una lista**

Un nodo de un árbol binario está en el nivel N si la ruta de la raíz al nodo tiene longitud N-1. El nodo raíz está en el nivel 1. Escriba un predicado atlevel / 3 para recopilar todos los nodos en un nivel dado de una lista.

% Atlevel (T, L, S): - S es la lista de nodos del árbol binario T en el nivel L

Usando atlevel / 3 es fácil construir un nivel de predicado order / 2 que crea la secuencia de orden de nivel de los nodos. Sin embargo, hay maneras más eficientes de hacer eso.

**4.12 ( * * ) Construir un árbol binario completo**

Un árbol binario *completo* con altura H se define como se presenta: Los niveles 1,2,3, ..., H-1 contienen el número máximo de nodos (es decir, 2 ** (i-1) en el nivel i, Comenzamos a contar los niveles de 1 en la raíz). En el nivel H, que puede contener menos que el número máximo posible de nodos, todos los nodos están "ajustados a la izquierda". Esto significa que en un árbol de nivel, todos los nodos internos vienen primero, las hojas vienen en segundo lugar, y los sucesores vacíos (los nil que no son realmente nodos!) Vienen en último lugar.

En particular, los árboles binarios completos se utilizan como estructuras de datos (o esquemas de direccionamiento) para montones.

Podemos asignar un número de dirección a cada nodo en un árbol binario completo enumerando los nodos en levelorder, comenzando en la raíz con el número 1. Al hacerlo, nos damos cuenta de que para cada nodo X con la dirección A se cumple la siguiente propiedad: De los sucesores izquierdo y derecho de X son 2 * A y 2 * A + 1, respectivamente, se supone que los sucesores existen. Este hecho se puede utilizar para construir elegantemente una estructura de árbol binario completa. Escribir un predicado complete_binary_tree / 2 con la siguiente especificación:

    % Complete_binary_tree (N, T): - T es un árbol binario completo con N nodos. (+)
    Pruebe su predicado de una manera apropiada.

**4.13 ( * * ) Diseño de un árbol binario (1)**

Dado un árbol binario como el término Prolog usual t (X, L, R) (o nil). Como una preparación para dibujar el árbol, se requiere un algoritmo de disposición para determinar la posición de cada nodo en una cuadrícula rectangular. Se pueden concebir varios métodos de disposición, uno de ellos se muestra en la siguiente ilustración.

![enter image description here](https://lh3.googleusercontent.com/-RdfyuqmXrcU/WMc_yYKBLRI/AAAAAAAAABc/ISczT9nEODgqXeT1cI9pmXJBeDGwAXv0QCLcB/s0/arbol1.gif "arbol1.gif")

En esta estrategia de disposición, la posición de un nodo v se obtiene mediante las dos reglas siguientes:

 - *X (v)* es igual a la posición del nodo v en el inorder
 - *Y (v)* es igual a la profundidad del nodo v en el árbol secuencia

Con el fin de almacenar la posición de los nodos, extendemos el término Prolog representando un nodo (y sus sucesores) como sigue:

% nil representa el árbol vacío (como de costumbre)
% t (W, X, Y, L, R) representa un árbol binario (no vacío) con raíz W "posicionada" en (X, Y) y subárboles L y R

Escribir un predicado layout_binary_tree / 2 con la siguiente especificación:

% Layout_binary_tree (T, PT): - PT es el árbol binario "posicionado" obtenido del árbol binario T. (+,?)

Pruebe su predicado de una manera apropiada.

**4.14 ( * * ) Diseño de un árbol binario (2)**

![enter image description here](https://lh3.googleusercontent.com/-O2bXDeeXyAM/WMdAtnNkpVI/AAAAAAAAABo/SmA1UkINKNYOewPAEA9p5u6yNmFEhkFhQCLcB/s0/arbol3.gif "arbol3.gif")

Un método de diseño alternativo se representa en la ilustración anterior. Averigüe las reglas y escriba el predicado Prolog correspondiente. Sugerencia: En un nivel dado, la distancia horizontal entre nodos vecinos es constante.

Utilice las mismas convenciones que en el problema 4.13 y pruebe su predicado de una manera apropiada.

**4.15 ( * * * ) Diseño de un árbol binario (3**)

![enter image description here](https://lh3.googleusercontent.com/-ybQmyN0LCT4/WMdBv8Aan4I/AAAAAAAAAB8/iBDKAr7iZOA7KnyHlnPlleld0ky1cY6fQCLcB/s0/arbol4.gif "arbol4.gif")

Sin embargo, otra estrategia de diseño se muestra en la ilustración anterior. El método proporciona una disposición muy compacta manteniendo una cierta simetría en cada nodo. Averigüe las reglas y escriba el predicado Prolog correspondiente. Sugerencia: Considere la distancia horizontal entre un nodo y sus nodos sucesores. ¿Qué tan apretado puede juntar dos subárboles para construir el árbol binario combinado?

Utilice las mismas convenciones que en el problema 4.13 y 4.14 y pruebe su predicado de una manera apropiada. Nota: Este es un problema difícil. ¡No te rindas demasiado pronto!

¿Qué plano le gusta más?

**4.16 ( * * ) Una representación de cadena de árboles binarios**

![enter image description here](https://lh3.googleusercontent.com/-aHeYBuVvQQU/WMdCIB85jpI/AAAAAAAAACE/AJoq6crURSIPxU89gOngbO_SaqibL55PwCLcB/s0/arbol5.gif "arbol5.gif")

Alguien representa árboles binarios como cadenas del siguiente tipo (ver ejemplo):

A (b (d, e), c (, f (g,)))

**a)** Escribe un predicado Prolog que genera esta representación de cadena, si el árbol se da como de costumbre (como nil o t (X, L, R)). Luego escribe un predicado que haga esto inverso; Es decir, dada la representación de la cadena, construya el árbol en la forma usual. Por último, combinar los dos predicados en un único predicado tree_string / 2 que se puede utilizar en ambas direcciones.

**b)** Escriba el mismo predicado tree_string / 2 usando listas de diferencias y un único predicado tree_dlist / 2 que realiza la conversión entre un árbol y una lista de diferencias en ambas direcciones.

Para simplificar, supongamos que la información en los nodos es una sola letra y no hay espacios en la cadena.

**4.17 ( * * ) Secuencias preordenadas e inordenadas de árboles binarios**

Consideramos árboles binarios con nodos que son identificados por letras minúsculas simples, como en el ejemplo del problema 4.16.

**a)** Escribir los predicados preorder / 2 e inorder / 2 que construyen la orden previa y la secuencia inorder de un árbol binario dado, respectivamente. Los resultados deben ser átomos, p. 'Abdecfg' para la secuencia de preorden del ejemplo en el problema 4.16.

**b)** ¿Se puede usar preorder / 2 de la parte problema a) en la dirección inversa; Es decir, dado una secuencia preordenada, construir un árbol correspondiente? Si no, hacer los arreglos necesarios.

**c)** Si se dan tanto la secuencia preordenada como la secuencia inordenada de los nodos de un árbol binario, entonces el árbol se determina sin ambigüedad. Escribe un predicado pre_in_tree / 3 que hace el trabajo.

**d)** Resolver los problemas a) a c) utilizando las listas de diferencias. ¡Guay! Utilice el predicado predefinido time / 1 para comparar las soluciones.

Qué sucede si el mismo carácter aparece en más de un nodo. Pruebe por ejemplo pre_in_tree (aba, baa, T).

**4.18 ( * * ) Representación en puntos de árboles binarios**

Consideramos de nuevo árboles binarios con nodos que se identifican mediante letras minúsculas simples, como en el ejemplo del problema 4.16. Dicho árbol puede ser representado por la secuencia preordenada de sus nodos en la que se insertan puntos (.) Donde se encuentra un subárbol vacío (nil) durante el recorrido del árbol. Por ejemplo, el árbol mostrado en el problema 4.16 se representa como 'abd..e..c.fg ...'. En primer lugar, trate de establecer una sintaxis (BNF o diagramas de sintaxis) y luego escribir un predicado tree_dotstring / 2 que realiza la conversión en ambas direcciones. Use las listas de diferencias.

[Soluciones-3](https://github.com/vml86/prologp/blob/master/SolucionesAB.md)

----------



_ _ _ _ _
