**6.Gráficos**
-------------
Las soluciones las puedes encontrar aquí

----------
**Una observación preliminar: El vocabulario en la teoría de los gráficos varía considerablemente. Algunos autores usan la misma palabra con diferentes significados. Algunos autores usan palabras diferentes para significar lo mismo. Espero que nuestras definiciones estén libres de contradicciones.**

**Un gráfico se define como un conjunto de *nodos* y un conjunto de *aristas*, donde cada borde es un par de nodos.**

Aquí hay varias maneras de representar gráficas en Prolog.

Un método representa cada borde por separado como una cláusula (hecho). En esta forma, el gráfico representado en el lado opuesto se representa como el siguiente predicado:

    edge(h,g).
    edge(k,f).
    edge(f,b).    
    ...

![enter image description here](https://lh3.googleusercontent.com/-73dPZ0oslAM/WMnzfWxxM6I/AAAAAAAAADo/Knrmf1LfeeIupa4FgLKGf4t2nrsMUFVIACLcB/s0/graph1.gif "graph1.gif")

Llamamos a esta ***forma de cláusula de borde***.

Obviamente, los nodos aislados no pueden ser representados. Otro método consiste en representar todo el gráfico como un objeto de datos. De acuerdo con la definición del gráfico como un par de dos conjuntos (nodos y aristas), podemos usar el siguiente término de Prolog para representar el ejemplo de gráfico anterior:

grafico([b,c,d,f,g,h,k],[e(b,c),e(b,f),e(c,f),e(f,k),e(g,h)])

Llamamos a esta forma de ***término-gráfico***. Tenga en cuenta que las listas se mantienen ordenadas, son realmente conjuntos, sin elementos duplicados. Cada borde aparece sólo una vez en la lista de bordes; Es decir, un borde de un nodo x a otro nodo y se representa como e (x, y), el término e (y, x) no está presente. **La forma del término-gráfico es nuestra representación por defecto**. En SWI-Prolog existen predicados predefinidos para trabajar con conjuntos.

Un tercer método de representación es asociar con cada nodo el conjunto de nodos que están adyacentes a ese nodo. Llamamos a esto  ***forma de lista de adyacencia***. En nuestro ejemplo:

[n(b,[c,f]), n(c,[b,f]), n(d,[]), n(f,[b,c,k]), ...]

Las representaciones que hemos introducido hasta ahora son términos Prolog y, por lo tanto, son adecuadas para el procesamiento automatizado, pero su sintaxis no es muy fácil de usar. Escribir los términos a mano es engorroso y propenso a errores. Podemos definir una notación más compacta y "amigable con el ser humano" como sigue: Un gráfico está representado por una lista de átomos y términos del tipo X-Y (es decir, functor '-' y aridad 2). Los átomos representan nodos aislados, los términos X-Y describen bordes. Si una X aparece como un punto final de un borde, se define automáticamente como un nodo. Nuestro ejemplo podría escribirse como:
[b-c, f-c, g-h, d, f-b, k-f, h-g]

Llamamos a esto la ***forma hombre-amistosa***. Como muestra el ejemplo, la lista no tiene que ser ordenada e incluso puede contener el mismo borde varias veces. Observe el nodo aislado d. (En realidad, los nodos aislados ni siquiera tienen que ser átomos en el sentido Prolog, pueden ser términos compuestos, como en d (3.75, azul) en lugar de d en el ejemplo).

Cuando los bordes están *dirigidos* los llamamos *arcos*. Éstos se representan por pares *ordenados*. Este grafo se denomina **gráfico dirigido** (o digrafo, para abreviar). Para representar un grafo dirigido, las formas discutidas anteriormente se modifican ligeramente. El gráfico de ejemplo que se muestra a continuación se representa de la siguiente manera:
*forma de cláusula-arco* 

    arc(s,u).
    arc(u,r).
    ...

*forma término-gráfico*

> dígrafo([r,s,t,u,v],[a(s,r),a(s,u),a(u,r),a(u,s),a(v,u)])

*Forma hombre-amigable*

    [s > r, t, u > r, s > u, u > s, v > u]

Finalmente, los gráficos y dígrafos pueden tener información adicional asociada a nodos y aristas (arcos). Para los nodos, esto no es un problema, ya que podemos reemplazar fácilmente los identificadores de un solo carácter por términos compuestos arbitrarios, como city ('London', 4711). Por otro lado, para los bordes tenemos que extender nuestra notación. Los gráficos con información adicional adjunta a los bordes se llaman gráficos etiquetados.

*Forma  de cláusula-arco* 
 

     arc(m,q,7).
     arc(p,q,9).
     arc(p,m,5).

 
*Forma término-gráfico*

    Dígrado ([k,m,p,q],[a(m,p,7),a(p,m,5),a(p,q,9)])

*Forma lista-adjancencia*

    [n(k,[]),n(m,[q/7]),n(p,[m/5,q/9]),n(q,[])]

Observe cómo la información de borde se ha empaquetado en un término con functor '/' y arity 2, junto con el nodo correspondiente.

*Forma hombre-amistosa*

    [p>q/9, m>q/7, k, p>m/5]

La notación de los gráficos etiquetados también se puede utilizar para los llamados **multi-gráficos**, donde se permite más de un borde (o arco) entre dos nodos dados.

**6.01 (* * *) Conversiones**

Escribir predicados para convertir entre las diferentes representaciones gráficas. Con estos predicados, todas las representaciones son equivalentes; Es decir, para los siguientes problemas siempre se puede elegir libremente la forma más conveniente. La razón por la que este problema está clasificado (***) no es porque es particularmente difícil, sino porque es un montón de trabajo para tratar con todos los casos especiales.

**6.02 (* *) Ruta de un nodo a otro**

Escriba una ruta de predicado (G, A, B, P) para encontrar una trayectoria acíclica P del nodo A al nodo B en el gráfico G. El predicado debe devolver todos los recorridos a través de retroceso.

**6.03 (*) Ciclo de un nodo dado**

Escribir un ciclo de predicado (G, A, P) para encontrar un camino cerrado (ciclo) P comenzando en un nodo dado A en el gráfico G. El predicado debe devolver todos los ciclos mediante retroceso.

**6.04 (* *) Construir todos los árboles de expansión**

Escriba un predicado s_tree (Gráfico, Árbol) para construir (por retroceso) todos los árboles que se extienden de un gráfico dado. Con este predicado, averiguar cuántos árboles que se extienden hay para el gráfico representado a la izquierda. Los datos de este ejemplo de gráfico se pueden encontrar en el archivo p6_04.dat. Cuando tenga una solución correcta para el predicado s_tree / 2, utilícelo para definir otros dos predicados útiles: is_tree (Graph) y is_connected (Graph). Ambas son tareas de cinco minutos!

![enter image description here](https://lh3.googleusercontent.com/-pZGBLlc_un0/WMn3PT7H_bI/AAAAAAAAAD4/aK59ItfTcIcT1jEFmlr_nFShbR8YrCo5QCLcB/s0/p83.gif "p83.gif")

**6.05 (* *) Construir el árbol de expansión mínimo**

Escriba un predicado ms_tree (Graph, Tree, Sum) para construir el árbol de expansión mínimo de un gráfico etiquetado dado. Sugerencia: Utilice el algoritmo de Prim. Una pequeña modificación de la solución de 6.04 hace el truco. Los datos del gráfico de ejemplo a la derecha se pueden encontrar en el archivo p6_05.dat.

![enter image description here](https://lh3.googleusercontent.com/-V2B2LAshaM4/WMn3aMuTncI/AAAAAAAAAEA/risQX68EhloNxsK4Kdvz2Jgqqs8k-1MaACLcB/s0/p84.gif "p84.gif")

**6.06 (* *) Gráfico isomorfismo**
Dos gráficas G1 (N1, E1) y G2 (N2, E2) son isomorfas si existe una bijección f: N1 -> N2 tal que para cualquier nodo X, Y de N1, X e Y son adyacentes si y solo si f X) yf (Y) son adyacentes.

Escribe un predicado que determina si dos gráficas son isomorfas. Sugerencia: Utilice una lista abierta para representar la función f.

**6.07 (* *) Grado del nodo y coloración del gráfico**

**a)** Escribe un grado de predicado (Gráfico, Nodo, Deg) que determina el grado de un nodo dado.
**b)** Escriba un predicado que genere una lista de todos los nodos de un gráfico ordenados según el grado decreciente.
**c)** Utilice el algoritmo de Welch-Powell para pintar los nodos de un gráfico de tal manera que los nodos adyacentes tengan colores diferentes.

**6.08 (* *) Profundidad-primer orden de recorrido transversal**

Escribe un predicado que genera una secuencia transversal de orden de profundidad-primer orden. Se debe especificar el punto de inicio y la salida debe ser una lista de nodos que se pueden alcanzar desde este punto de inicio (en profundidad-primer orden).

**6.09 (* *) Componentes conectados**
Escribe un predicado que divide un gráfico en sus componentes conectados.

**6.10 (* *) Gráficos bipartitos**
Escribe un predicado que averigüe si un gráfico dado es bipartito.

**6.11 (* * *) Generar gráficos simples K-regulares con N nodos**
En un gráfico K-regular todos los nodos tienen un grado de K; Es decir, el número de aristas incidentes en cada nodo es K. ¿Cuántos gráficos (no isomórficos) 3-regulares con 6 nodos hay?

Véase también la tabla de resultados en p6_11.txt.

----------



_ _ _ _ _