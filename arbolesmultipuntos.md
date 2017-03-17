
**5.Arboles Multipuntos**
--------------
Las soluciones pueden ser encontradas [aqui](https://github.com/vml86/prologp/blob/master/ArbolesMultirutas.md)


----------


**Un árbol de múltiples vías se compone de un elemento raíz y un conjunto (posiblemente vacío) de sucesores que son árboles de múltiples vías ellos mismos. Un árbol de múltiples vías nunca está vacío. El conjunto de árboles sucesores a veces se llama un bosque.**

![enter image description here](https://lh3.googleusercontent.com/-mU4fsiH__N4/WMdW-lkF_JI/AAAAAAAAACw/VcVCCHZQRFIcFm7CHjGuldfRVAP9ltDwACLcB/s0/p70.gif "p70.gif")

En Prolog representamos un árbol de múltiples vías por un término t (X, F), donde X denota el nodo raíz y F denota el bosque de árboles sucesores (una lista Prolog). Por lo tanto, el árbol de ejemplo representado de manera opuesta está representado por el siguiente término de Prolog:

    T = t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])

**5.01 (*) Compruebe si un término dado representa un árbol de múltiples vías**
Escribir un predicado istree / 1 que tiene éxito si y sólo si su argumento es un término Prolog que representa un árbol de múltiples vías.

> **Ejemplo:**

    (T, t), t (a, [t (f, [t (g, [])]), t (c, [] Unesdoc.unesco.org unesdoc.unesco.org
    Sí

**5.02 (*) Cuente los nodos de un árbol multipunto**
Escriba un predicado nnodes / 1 que cuenta los nodos de un árbol multiway dado.

> **Ejemplo:**

    ? - nnodes (t (a, [t (f, [])]), N).
    N = 2

Escriba otra versión del predicado que permita un patrón de flujo (o, i).

**5.03 (* *) Construcción de un árbol a partir de una cadena de nodos**

Suponemos que los nodos de un árbol de múltiples vías contienen caracteres individuales. En la secuencia de orden de profundidad de sus nodos, se ha insertado un carácter especial ^ cuando, durante el recorrido del árbol, el movimiento es un retroceso al nivel anterior.

Por esta regla, el árbol en la figura de abajo está representado por:

     afg ^^ c ^ bd ^ e ^^^

Defina la sintaxis de la cadena y escriba un árbol de predicados (Cadena, Árbol) para construir el Árbol cuando se da la Cadena. Trabajar con átomos (en lugar de cadenas). Haga que su predicado funcione en ambas direcciones.
![enter image description here](https://lh3.googleusercontent.com/-hamkoDVuqvc/WMdXVYL9pmI/AAAAAAAAAC4/inyhS-4O32MZz6o5LdAHb5XfGSAzLsdSQCLcB/s0/p70+%25281%2529.gif "p70 &#40;1&#41;.gif")

**5.04 (*) Determine la longitud de la ruta interna de un árbol**

Definimos la longitud de trayecto interno de un árbol de múltiples vías como la suma total de las longitudes de trayecto desde la raíz a todos los nodos del árbol. Por esta definición, el árbol en la figura del problema 5.03 tiene una longitud de trayectoria interna de 9.

Escribe un predicado ipl (Árbol, IPL) para el patrón de flujo (+, -).

**5.05 (*) Construya la secuencia de orden ascendente de los nodos de árbol**
Escribir un predicado bottom_up (Tree, Seq) que construye la secuencia ascendente de los nodos del árbol multiway árbol. Seq debe ser una lista Prolog.

¿Qué sucede si ejecuta sus contraseñas de predicado?

**5.06 (* *) Representación en árbol similar a Lisp**
Hay una notación particular para los árboles multiway en Lisp. Lisp es un prominente lenguaje de programación funcional, que se utiliza principalmente para los problemas de inteligencia artificial. Como tal, es uno de los principales competidores de Prolog. En Lisp casi todo es una lista, como en Prolog todo es un término.

Las siguientes imágenes muestran cómo se representan las estructuras de árbol multipunto en Lisp.

![enter image description here](https://lh3.googleusercontent.com/-JliG1d06g4A/WMdXw3uTvKI/AAAAAAAAADA/vgJF70oORFgJLXi37iIJtSxS7Be4Kl6rgCLcB/s0/p73.png "p73.png")

Tenga en cuenta que en la notación "lispy" un nodo con sucesores (hijos) en el árbol es siempre el primer elemento en una lista, seguido por sus hijos. La representación "lispy" de un árbol multiway es una secuencia de átomos y paréntesis '(' y ')', que llamaremos colectivamente "tokens". Podemos representar esta secuencia de tokens como una lista Prolog;

 p.ej. La expresión lispy (a (b c)) podría ser representada como la lista Prolog ['(', a, '(', b, c, ')', ')']. Escriba un predicado tree_ltl (T, LTL) que construye la "lista de tokens lispy" LTL si el árbol se da como término T en la notación Prolog usual.

> **Ejemplo:**

    ? - tree_ltl (t (a, [t (b, []), t (c, [])]), LTL).
    LTL = ['(', a, '(', b, c, ')', ')']

Como un segundo ejercicio, aún más interesante intentar reescribir tree_ltl / 2 de una manera que la conversión inversa es también posible:
 Dada la lista LTL, construir el árbol Prolog T. Utilizar las listas de diferencias. .



