**% 4.01 Escribir un predicado istree / 1 el cual tendra éxito si y sólo si su argumento Es un término Prolog que representa un árbol binario.**
% Istree (T): - T es un término que representa un árbol binario (i), (o)

    istree(nil).
    istree(t(_,L,R)) :- istree(L), istree(R).

% Casos de prueba (también se pueden utilizar para otros problemas de árbol binario).

> tree(1,t(a,t(b,t(d,nil,nil),t(e,nil,nil)),t(c,nil,t(f,t(g,nil,nil),nil)))).
> 
> tree(2,t(a,nil,nil)).
> 
> tree(3,nil).

**% 4,02 ( * * ) Construir árboles binarios completamente equilibrados para un dado**
% Número de nodos.
% Cbal_tree (N, T): - T es un árbol binario completamente equilibrado con N nodos.
% (Entero, árbol) (+,?)

    cbal_tree(0,nil) :- !.
    cbal_tree(N,t(x,L,R)) :- N > 0,
    N0 is N - 1, 
    N1 is N0//2, N2 is N0 - N1,
    distrib(N1,N2,NL,NR),
    cbal_tree(NL,L), cbal_tree(NR,R).

    distrib(N,N,N,N) :- !.
    distrib(N1,N2,N1,N2).
    distrib(N1,N2,N2,N1).

**% 4,03 ( * * ) Árboles binarios simétricos**

% Llamemos a un árbol binario simétrico si se puede dibujar un vertical
% A través del nodo raíz y luego el subárbol derecho es el espejo
% imagen del subárbol izquierdo.
% Escriba un predicado simétrico / 1 para comprobar si un binario dado
% es àrbol  simétrico. Sugerencia: Escribe un predicado espejo / 2 primero para comprobar
% Si un árbol es la imagen especular de otro.
% Simétrico (T): - el árbol binario T es simétrico.

    symmetric(nil).

    symmetric(t(_,L,R)) :- mirror(L,R).

    mirror(nil,nil).

    mirror(t(_,L1,R1),t(_,L2,R2)) :- mirror(L1,R2), mirror(R1,L2).

**% 4.04 ( * * ) Árboles binarios de búsqueda (diccionarios)**

% Utilice el predicado add / 3, desarrollado en el capítulo 4 del curso,
% para escribir un predicado para construir un árbol de búsqueda binario
% de una lista de números enteros. A continuación, utilice este predicado para probar
% la solución del problema P56

    : - ensure_loaded (p4_03).

% Add (X, T1, T2): - el diccionario binario T2 se obtiene por
% Que añade el elemento X al diccionario binario T1
% (Elemento, binario-diccionario, binario-diccionario) (i, i, o)

> add(X,nil,t(X,nil,nil)).
> 
> add(X,t(Root,L,R),t(Root,L1,R)) :- X @< Root, add(X,L,L1).
> 
> add(X,t(Root,L,R),t(Root,L,R1)) :- X @> Root, add(X,R,R1).
> 
> construct(L,T) :- construct(L,T,nil).
> 
> construct([],T,T).
> 
> construct([N|Ns],T,T0) :- add(N,T0,T1), construct(Ns,T,T1).
> 
> 
> test_symmetric(L) :- construct(L,T), symmetric(T).

**% 4.05 ( * * ) Generar y probar el paradigma**

% Aplicar el paradigma de generar y probar para construir todas las simétricas,
arboles binarios completamente equilibrados con un número dado de nodos.


    :- ensure_loaded(p4_02).
    :- ensure_loaded(p4_03).


> sym_cbal_tree(N,T) :- cbal_tree(N,T), symmetric(T).
> 
> sym_cbal_trees(N,Ts) :- setof(T,sym_cbal_tree(N,T),Ts).

    investigate(A,B) :-
    between(A,B,N),
    sym_cbal_trees(N,Ts), length(Ts,L),
    writef('%w   %w',[N,L]), nl,
    fail.
    investigate(_,_).

**% 4.06 ( * * ) Construir árboles binarios de altura equilibrada**

% En un árbol binario de altura equilibrada, la propiedad siguiente es válida para
% cada nodo: La altura de su subárbol izquierdo y la altura de
% su subárbol derecho son casi iguales, lo que significa su
% diferencia no es mayor que uno.
% Escribir un predicado hbal_tree / 2 para construir altura equilibrada
% Árboles binarios para una altura determinada. El predicado debe
% generar todas las soluciones mediante backtracking. Ponga la letra 'x'
% como información en todos los nodos del árbol.

% Hbal_tree (D, T): - T es un árbol binario de altura equilibrada con profundidad T


    hbal_tree(0,nil) :- !.
    hbal_tree(1,t(x,nil,nil)) :- !.
    hbal_tree(D,t(x,L,R)) :- D > 1,
    D1 is D - 1, D2 is D - 2,
    distr(D1,D2,DL,DR),
    hbal_tree(DL,L), hbal_tree(DR,R).

    distr(D1,_,D1,D1).
    distr(D1,D2,D1,D2).
    distr(D1,D2,D2,D1).

**% 4.07 (* *) Construir árboles binarios con equilibrio de altura con un número dado de nodos**

    : - ensure_loaded (p4_06).

% MinNodos (H, N): - N es el número mínimo de nodos en una relación de altura
Árbol binario de altura H
% (Entero, entero) (+,?)

    MinNodos (0,0): -!.
    MinNodos (1,1): -.
    MinNodos (H, N): - H> 1,
    H _ {1} es H - 1, H2 es H - 2,
    MinNodos (H1, N1), minNodos (H2, N2),
    N es 1 + N ^ {1} + N ^ {2}.

% MaxNodos (H, N): - N es el número máximo de nodos en una altura equilibrada
árbol binario de altura H
% (Entero, entero) (+,?)

    MaxNodos (H, N): - N es 2 ** H - 1.

% MinAltura (N, H): - H es la altura mínima de una altura equilibrada
% Árbol binario con N nodos
% (Entero, entero) (+,?)

> MinHeight (0,0): -. MinHeight (N, H): - N > 0, N1 es N // 2, minHeight
> (N1, H1), H es H1 + 1.

% MaxHeight (N, H): - H es la altura máxima de una altura equilibrada
% Árbol binario con N nodos
% (Entero, entero) (+,?)

> MaxHeight (N, H): - maxHeight (N, H, 1,1).
> 
> MaxHeight (N, H, H1, N1): - N1 > N,., H es H1 - 1. MaxHeight (N, H,
> H1, N1): - N1 = < N, H2 es H1 + 1, minNodos (H2, N2), maxHeight (N, H, H2, N2).

% Hbal_tree_nodes (N, T): - T es un árbol binario equilibrado en altura con N nodos.

    Hbal_tree_nodes (N, T): -
    MinHeight (N, Hmin), maxHeight (N, Hmax),
    Entre (Hmin, H _ {max}, H),
    Hbal_tree (H, T), nodos (T, N).

% El predicado siguiente es del curso (capítulo 4)

% Nodos (T, N): - el árbol binario T tiene N nodos
% (Árbol, entero); (yo,*)

    nodes(nil,0).
    nodes(t(_,Left,Right),N) :-
    nodes(Left,NLeft),
    nodes(Right,NRight),
    N is NLeft + NRight + 1.

% Count_hbal_trees (N, C): - hay C diferente de altura equilibrada binario% de árboles con N nodos.

count_hbal_trees(N,C):-setof(T,hbal_tree_nodes(N,T),Ts), length(Ts,C). 

**% 4.08 (*) Cuente las hojas de un árbol binario**

    : - ensure_loaded (p4_01).

% Count_leaves (T, N): - el árbol binario T tiene N hojas

> Count_leaves (nil, 0). Count_leaves (t (_, nil, nil), 1). Count_leaves
> (t (_, L, nil), N): - L = t (_, _, _), count_leaves (L, N).
> Count_leaves (t (_, nil, R), N): - R = t (_, _, _), count_leaves (R,
> N). (T, L, R), N): - L = t (_, _, _), R = t (_, _, _),  Count_leaves
> (L, NL), count_leaves (R, NR), N es NL + NR.

% La solución anterior funciona en los patrones de flujo (i, o) y (i, i)
% Sin cortar y produce un solo resultado correcto. Usando un corte
% Podemos obtener el mismo resultado en un programa mucho más corto, como esto:

> Count_leaves1 (nil, 0). Count_leaves1 (t (_, nil, nil), 1): -.
> Count_leaves1 (t (_, L, R), N): - Count_leaves1 (L, NL), count_leaves1
> (R, NR), N es NL + NR.

% Para el patrón de flujo (o, i) véase el problema 4.09).

**% 4.09 (*) Recopila las hojas de un árbol binario en una lista**

    : - ensure_loaded (p4_01).

% Hojas (T, S): - S es la lista de las hojas del árbol binario T

> leaves(nil,[]).
> 
> leaves(t(X,nil,nil),[X]).
> 
> leaves(t(_,L,nil),S) :- L = t(_,_,_), leaves(L,S).
> 
> leaves(t(_,nil,R),S) :- R = t(_,_,_), leaves(R,S).
> 
> leaves(t(_,L,R),S) :- L = t(_,_,_), R = t(_,_,_),
> 
> leaves(L,SL), leaves(R,SR), append(SL,SR,S).

% La solución anterior funciona en los patrones de flujo (i, o) y (i, i)
% Sin cortar y produce un solo resultado correcto. Usando un corte
% Podemos obtener el mismo resultado en un programa mucho más corto, como esto:

    leaves1(nil,[]).
    leaves1(t(X,nil,nil),[X]) :- !.
    leaves1(t(_,L,R),S) :- 
    leaves1(L,SL), leaves1(R,SR), append(SL,SR,S).

% Para escribir un predicado que funcione en el patrón de flujo (o, i)
% Es un problema más difícil, porque el uso de append / 3 en
% el patrón de flujo (o, o, i) siempre genera una lista vacía
% como primera solución y el resultado es una recursión infinita
% a lo largo del subárbol izquierdo del árbol binario generado.
% Una posible solución es el siguiente truco: nosotros sucesivamente
% construye estructuras de árbol binario para un número dado de nodos
% y llenar los nodos hoja con los elementos de la lista de hojas.
% A continuación, incrementar el número de nodos de árbol sucesivamente,
% y así.

% Nnodes (T, N): - T es un árbol binario con N nodos (o, i)    

    nnodes(nil,0) :- !.
    nnodes(t(_,L,R),N) :- N > 0, N1 is N-1, 
    between(0,N1,NL), NR is N1-NL,
    nnodes(L,NL), nnodes(R,NR).

% Hojas2 (T, S): - S es la lista de hojas del árbol T (o, i)
eaves2(T,S) :- leaves2(T,S,0).

    leaves2(T,S,N) :- nnodes(T,N), leaves1(T,S).
    leaves2(T,S,N) :- N1 is N+1, leaves2(T,S,N1).
% OK, esto fue dificultoso (**)

**% 4.10 (*) Recopile los nodos internos de un árbol binario en una lista**

    : - ensure_loaded (p4_01).

% Internals (T, S): - S es la lista de nodos internos del árbol binario T.

> internals(nil,[]). internals(t(_,nil,nil),[]).
> internals(t(X,L,nil),[X|S]) :- L = t(_,_,_), internals(L,S).
> internals(t(X,nil,R),[X|S]) :- R = t(_,_,_), internals(R,S).
> internals(t(X,L,R),[X|S]) :- L = t(_,_,_), R = t(_,_,_), 
> internals(L,SL), internals(R,SR), append(SL,SR,S).


% La solución anterior funciona en los patrones de flujo (i, o) y (i, i)
% Sin cortar y produce un solo resultado correcto. Usando un corte
% Podemos obtener el mismo resultado en un programa mucho más corto, como esto:

    internals1(nil,[]).
    internals1(t(_,nil,nil),[]) :- !.
    internals1(t(X,L,R),[X|S]) :- 

    internals1(L,SL), internals1(R,SR), append(SL,SR,S).

% Para el patrón de flujo (o, i) existe el siguiente
Solución elegante:

    internals2(nil,[]).
    internals2(t(X,L,R),[X|S]) :- 
    append(SL,SR,S), internals2(L,SL), internals2(R,SR).

**% 4.11 (*) Recopile los nodos de un árbol binario en un nivel dado en una lista**

    : - ensure_loaded (p4_01).

% Atlevel (T, D, S): - S es la lista de nodos del árbol binario T en el nivel D
% (I, i, o)

    atlevel(nil,_,[]).
    atlevel(t(X,_,_),1,[X]).
    atlevel(t(_,L,R),D,S) :- D > 1, D1 is D-1,

    atlevel(L,D1,SL), atlevel(R,D1,SR), append(SL,SR,S).


%Lo siguiente es una solución rápida y sucia para el
% Secuencia de orden de nivel

> levelorder(T,S) :- levelorder(T,S,1).
> 
> levelorder(T,[],D) :- atlevel(T,D,[]), !. levelorder(T,S,D) :-
> atlevel(T,D,SD), D1 is D+1, levelorder(T,S1,D1), append(SD,S1,S).

**% 4.12 ( * * ) Construir un árbol binario completo**

% Un árbol binario completo con altura H se define de la siguiente manera:
% Los niveles 1,2,3, ..., H-1 contienen el número máximo de nodos
% (es decir 2 ** (i-1) en el nivel i, tenga en cuenta que comenzamos a contar el
% nivel de 1 en la raíz). En el nivel H, que puede contener menos
% que el número máximo posible de nodos, todos los nodos son
% "ajustados a la izquierda". Esto significa que en una trayectoria de árbol de nivel
% Todos los nodos internos vienen primero, las hojas vienen en segundo lugar, y
% sucesores vacíos (los nils que no son realmente nodos!)
% vienen por último. Se utilizan árboles binarios completos para montones.

    : - ensure_loaded (p4_04).

% Complete_binary_tree (N, T): - T es un árbol binario completo con
% N nodos. (+)

complete_binary_tree(N,T) :- complete_binary_tree(N,T,1).

    complete_binary_tree(N,nil,A) :- A > N, !.
    complete_binary_tree(N,t(_,L,R),A) :- A =< N,
    AL is 2 * A, AR is AL + 1,
    complete_binary_tree(N,L,AL),
    complete_binary_tree(N,R,AR).

Esta fue la solución del ejercicio. Lo que sigue es una aplicación
% De este resultado.

% Definimos un montón como un montón de términos (N, T) donde N es el número de elementos
% Y T un árbol binario completo (en el sentido usado arriba).

% El uso conservador de un montón es primero declararlo con un predicado
% Declare_heap / 2 y luego usarlo con un predicado element_at / 3.

% Declare_heap (H, N): -
% Declaran que H es un montón con un número fijo N de elementos

> Declare_heap (heap (N, T), N): - complete_binary_tree (N, T).

% Element_at (H, K, X): - X es el elemento en la dirección K en el montón H.
% El primer elemento tiene dirección 1.

%  (+,+,?)

> element_at(heap(_,T),K,X) :-   binary_path(K,[],BP),
> element_at_path(T,BP,X).
> 
> binary_path(1,Bs,Bs) :- !. binary_path(K,Acc,Bs) :- K > 1,  B is K /\
> 1, K1 is K >> 1, binary_path(K1,[B|Acc],Bs).
> 
> element_at_path(t(X,_,_),[],X) :- !.
> element_at_path(t(_,L,_),[0|Bs],X) :- !, element_at_path(L,Bs,X).
> element_at_path(t(_,_,R),[1|Bs],X) :- element_at_path(R,Bs,X).

% Podemos transformar listas en montones y viceversa con lo siguiente
% Predicado útil:

% List_heap (L, H): - transforma una lista en un montón (limitado) y viceversa.

> list_heap(L,H) :- is_list(L), list_to_heap(L,H).
> list_heap(L,heap(N,T)) :- integer(N), fill_list(heap(N,T),N,1,L).
> 
> list_to_heap(L,H) :-  length(L,N), declare_heap(H,N),
> fill_heap(H,L,1).
> 
> fill_heap(_,[],_). fill_heap(H,[X|Xs],K) :- element_at(H,K,X), K1 is
> K+1, fill_heap(H,Xs,K1).
> 
> fill_list(_,N,K,[]) :- K > N. fill_list(H,N,K,[X|Xs]) :- K =< N, 
> element_at(H,K,X), K1 is K+1, fill_list(H,N,K1,Xs).

% Sin embargo, un uso más agresivo es * no * para definir el montón en el
% Comenzando, pero para usarlo como una estructura de datos parcialmente instanciada.
% Usado de esta manera, el número de elementos en el montón es ilimitado.
% Este es Power-Prolog!

Intente lo siguiente e investigue exactamente qué sucede.
%? - element_at (H, 5, alfa), element_at (H, 2, beta), element (H, 5, A)

% Sección de prueba. Suponga que tiene N elementos en una lista que debe ser mirado
% M veces en un orden aleatorio.

    test1(N,M) :-
    length(List,N), lookup_list(List,N,M).

    lookup_list(_,_,0) :- !.
    lookup_list(List,N,M) :- 
    K is random(N)+1,% determine direccion aleatoria
    nth1(K,List,_),  % buscar y lanzar
    M1 is M-1,
    lookup_list(List,N,M1).

%? - tiempo (test1 (100,100000)).
% 1,384,597 inferencias en 3.98 segundos (347889 Lips)
%? - tiempo (test1 (500,100000)).
% 4,721,902 inferencias en 13.82 segundos (341672 Lips)
%? - tiempo (test1 (10000,100000)).
% 84,016,719 inferencias en 277,51 segundos (302752 labios)

    test2(N,M) :-
    declare_heap(Heap,N), 
    lookup_heap(Heap,N,M).

>    lookup_heap(_,_,0) :- !. lookup_heap(Heap,N,M) :-     K is
>      random(N)+1,       % determine a random address   
>      element_at(Heap,K,_),   % look up and throw away    M1 is M-1,   
>      lookup_heap(Heap,N,M1).

%? - tiempo (test2 (100,100000)).
% 3,002,061 inferencias en 7,81 segundos (384387 Lips)
%? - tiempo (test2 (500,100000)).
% 4,097,961 inferencias en 10.75 segundos (381206 labios)
%? - tiempo (test2 (10000,100000)).
% 6,366,206 inferencias en 19.16 segundos (332265 Lips)

% Conclusión: En este escenario, para listas de más de 500 elementos
% Es más eficiente usar un montón.

**% 4.13 ( * * ) Diseño de un árbol binario (1)**

Dado un árbol binario como el término Prolog usual t (X, L, R) (o nil).
Como una preparación para dibujar el árbol, un algoritmo de diseño es
% necesario para determinar la posición de cada nodo en un rectángulo
% cuadrícula. Se pueden concebir varios métodos de disposición, uno de ellos
% el seguimiento:
% La posición de un nodo v se obtiene mediante las dos reglas siguientes:
% X (v) es igual a la posición del nodo v en la secuencia de orden
% Y (v) es igual a la profundidad del nodo v en el árbol

% Con el fin de almacenar la posición de los nodos, extendemos el Prolog
% Término que representa un nodo (y sus sucesores) como se muestra a continuación :
% Nil representa el árbol vacío (como de costumbre)
% T (W, X, Y, L, R) representa un árbol binario (no vacío) con raíz
% W se posiciona en (X, Y), y los subárboles L y R
% Escribir un predicado layout_binary_tree / 2:

% Layout_binary_tree (T, PT): - PT es el binario "positionned"
% Árbol obtenido del árbol binario T. (+,?) O (?, +)

> :- ensure_loaded(p4_04). % for test layout_binary_tree(T,PT) :-
> layout_binary_tree(T,PT,1,_,1).


% Layout_binary_tree (T, PT, In, Out, D): - T y PT como en layout_binary_tree / 2;
% In es la posición en la secuencia inorder donde el árbol T (o PT)
% Comienza, Out es la posición después del último nodo de T (o PT) en el
% En secuencia de orden. D es la profundidad de la raíz de T (o PT).
 \ Vskip1.000000 \ baselineskip \ vskip1.000000 \ baselineskip \
 vskip1.000000 \ baselineskip \ vskip1.000000 \ baselineskip \
 vskip1.000000 \ baselineskip \ vskip1.000000 \

> layout_binary_tree(nil,nil,I,I,_).
> layout_binary_tree(t(W,L,R),t(W,X,Y,PL,PR),Iin,Iout,Y) :-     Y1 is Y
> + 1,    layout_binary_tree(L,PL,Iin,X,Y1),     X1 is X + 1,    layout_binary_tree(R,PR,X1,Iout,Y1).

% De prueba (ver ejemplo dado en la descripción del problema):
%? - constructo ([n, k, m, c, a, h, g, e, u, p, s, q], T), layout_binary_tree (T, PT).

**% 4.14 ( * * ) Diseño de un árbol binario (2)**
%
% Véase el problema 4.13 para las convenciones.
%
% La posición de un nodo v se obtiene mediante las siguientes reglas:
% (1) y (v) es igual a la profundidad del nodo v en el árbol
% (2) si D denota la profundidad del árbol (es decir, el número de
% De los niveles poblados), la distancia horizontal entre
% Nodos en el nivel i (contados desde la raíz, comenzando con 1)
% Es igual a 2 ** (D-i + 1). El nodo más a la izquierda del árbol
% Está en la posición 1.

% Layout_binary_tree2 (T, PT): - PT es el binario "posicionado"
% Árbol obtenido del árbol binario T. (+,?)

 % : - ensure_loaded (p4_04).Para la prueba

> layout_binary_tree2(nil,nil) :- !.  layout_binary_tree2(T,PT) :-    
> hor_dist(T,D4), D is D4//4, x_pos(T,X,D),    
> layout_binary_tree2(T,PT,X,1,D).

% Hor_dist (T, D4): - D4 es cuatro veces la distancia horizontal entre el
% Nodo raíz de T y su sucesor (s) (si existe).
%    (+,-)

    hor_dist(nil,1).
    hor_dist(t(_,L,R),D4) :- 
    hor_dist(L,D4L), 
    hor_dist(R,D4R),
    D4 is 2 * max(D4L,D4R).

% X_pos (T, X, D): - X es la posición horizontal del nodo raíz de T
% Con respecto al sistema de coordenadas de imagen. D es la horizontal
% De distancia entre el nodo raíz de T y su sucesor (s) (si existe).
Unesdoc.unesco.org unesdoc.unesco.org

> x_pos(t(_,nil,_),1,_) :- !. x_pos(t(_,L,_),X,D) :- D2 is D//2,
> x_pos(L,XL,D2), X is XL+D.

% Layout_binary_tree2 (T, PT, X, Y, D): - T y PT como en layout_binary_tree / 2;
% D es la distancia horizontal entre el nodo raíz de T y
% De su (s) sucesor (es) (si existe). X, Y son las coordenadas del nodo raíz.
%    (+,-,+,+,+)

    layout_binary_tree2(nil,nil,_,_,_).
    layout_binary_tree2(t(W,L,R),t(W,X,Y,PL,PR),X,Y,D) :- 
    Y1 is Y + 1,
    Xleft is X - D,
    D2 is D//2,
    layout_binary_tree2(L,PL,Xleft,Y1,D2), 
    Xright is X + D,
    layout_binary_tree2(R,PR,Xright,Y1,D2).
% Prueba (ver ejemplo dado en la descripción del problema):
%? - constructo ([n, k, m, c, a, e, d, g, u, p, q], T), layout_binary_tree2 (T, PT).

**% 4.15 ( * * * ) Diseño de un árbol binario (3)**

% Véase el problema 4.13 para las convenciones.

% La posición de un nodo v se obtiene mediante las siguientes reglas:
% (1) y (v) es igual a la profundidad del nodo v en el árbol
% (2) para determinar las posiciones horizontales de los nodos que
% Construyen "contornos" para cada subárbol y los cambian juntos
% Horizontalmente lo más cerca posible. Sin embargo, mantenemos la
% De simetría en cada nodo; Es decir, la distancia horizontal entre
% Un nodo y la raíz de su subárbol izquierdo es la misma que entre
% Y la raíz de su subárbol derecho.
% El "contorno" de un árbol es una lista de términos c (Xleft, Xright) que
% Dan la posición horizontal de los nodos más externos del árbol
% En cada nivel, con relación a la posición raíz. En el ejemplo
% Dado en la descripción del problema, el "contorno" del árbol con
% Raíz k sería [c (-1,1), c (-2,0), c (-1,1)]. Tenga en cuenta que la primera
El elemento% en la lista "contorno" se deriva de la posición de
% De los nodos c y m.

% Layout_binary_tree3 (T, PT): - PT es el binario "posicionado"
% Árbol obtenido del árbol binario T. (+,?)

% : - ensure_loaded (p4_04).  Para la prueba

    layout_binary_tree3(nil,nil) :- !. 
    layout_binary_tree3(T,PT) :-

>    contour_tree(T,CT),      % construct the "contour" tree CT    CT =
> t(_,_,_,Contour),    mincont(Contour,MC,0),   % find the position of
> the leftmost node    Xroot is 1-MC,   
> layout_binary_tree3(CT,PT,Xroot,1).

    contour_tree(nil,nil).
    contour_tree(t(X,L,R),t(X,CL,CR,Contour)) :- 
    contour_tree(L,CL),
    contour_tree(R,CR),
    combine(CL,CR,Contour).

    combine(nil,nil,[]).
    combine(t(_,_,_,CL),nil,[c(-1,-1)|Cs]) :- shift(CL,-1,Cs).
    combine(nil,t(_,_,_,CR),[c(1,1)|Cs]) :- shift(CR,1,Cs).
    combine(t(_,_,_,CL),t(_,_,_,CR),[c(DL,DR)|Cs]) :-
    maxdiff(CL,CR,MD,0), 
    DR is (MD+2)//2, DL is -DR,
    merge(CL,CR,DL,DR,Cs).

    shift([],_,[]).
    shift([c(L,R)|Cs],S,[c(LS,RS)|CsS]) :-
    LS is L+S, RS is R+S, shift(Cs,S,CsS).

    maxdiff([],_,MD,MD) :- !.
    maxdiff(_,[],MD,MD) :- !.
    maxdiff([c(_,R1)|Cs1],[c(L2,_)|Cs2],MD,A) :- 
    A1 is max(A,R1-L2),
    maxdiff(Cs1,Cs2,MD,A1).

    merge([],CR,_,DR,Cs) :- !, shift(CR,DR,Cs).
    merge(CL,[],DL,_,Cs) :- !, shift(CL,DL,Cs).
    merge([c(L1,_)|Cs1],[c(_,R2)|Cs2],DL,DR,[c(L,R)|Cs]) :-
    L is L1+DL, R is R2+DR,
    merge(Cs1,Cs2,DL,DR,Cs).

    mincont([],MC,MC).
    mincont([c(L,_)|Cs],MC,A) :- 
    A1 is min(A,L), mincont(Cs,MC,A1).

    layout_binary_tree3(nil,nil,_,_).
    layout_binary_tree3(t(W,nil,nil,_),t(W,X,Y,nil,nil),X,Y) :- !. 
    layout_binary_tree3(t(W,L,R,[c(DL,DR)|_]),t(W,X,Y,PL,PR),X,Y) :- 
    Y1 is Y + 1,
    Xleft is X + DL,
    layout_binary_tree3(L,PL,Xleft,Y1), 
    Xright is X + DR,
    layout_binary_tree3(R,PR,Xright,Y1).
% De prueba (ver ejemplo dado en la descripción del problema):
% \ Alpha - constructo ([n, k, m, c, a, e, d, g, u, p, q], T), layout_binary_tree3 (T, PT).


**% 4.16a ( * * ) Una representación de cadena de árboles binarios**

% La representación de cadena tiene la siguiente sintaxis:
% <Árbol> :: = | < Letter > < subtrees >
% < Subtrees > :: = | '(' <Árbol> ',' <árbol> ')'
% De acuerdo con esta sintaxis, un nodo hoja (con la letra x) podría
% Estar representado por x (,) y no solo por el solo caracter x.
% Sin embargo, evitaremos esto al generar la cadena
% De representación.

> tree_string(T,S) :- nonvar(T), !, tree_to_string(T,S). 
> tree_string(T,S) :- nonvar(S), string_to_tree(S,T). 
> tree_to_string(T,S) :- tree_to_list(T,L), atom_chars(S,L).

    tree_to_list(nil,[]).
    tree_to_list(t(X,nil,nil),[X]) :- !.
    tree_to_list(t(X,L,R),[X,'('|List]) :- 
    tree_to_list(L,LsL),
    tree_to_list(R,LsR),
    append(LsL,[','],List1),
    append(List1,LsR,List2),
    append(List2,[')'],List).

    string_to_tree(S,T) :- atom_chars(S,L), list_to_tree(L,T).
    
    list_to_tree([],nil).
    list_to_tree([X],t(X,nil,nil)) :- char_type(X,alpha).
    list_to_tree([X,'('|List],t(X,Left,Right)) :- char_type(X,alpha),
    append(List1,[')'],List),
    append(LeftList,[','|RightList],List1),
    list_to_tree(LeftList,Left),
    list_to_tree(RightList,Right).
    
**% 4.16b ( * * ) Una representación de cadena de árboles binarios**

Solución más elegante usando listas de diferencias.

> tree_string(T,S) :- nonvar(T), tree_dlist(T,L-[]), !, atom_chars(S,L).
> tree_string(T,S) :- nonvar(S), atom_chars(S,L), tree_dlist(T,L-[]).


% Tree_dlist / 2 hace el truco en ambas direcciones!

    tree_dlist(nil,L-L).
    tree_dlist(t(X,nil,nil),L1-L2) :- 
    letter(X,L1-L2).
    tree_dlist(t(X,Left,Right),L1-L7) :- 
    letter(X,L1-L2), 
    symbol('(',L2-L3),
    tree_dlist(Left,L3-L4),
    symbol(',',L4-L5),
    tree_dlist(Right,L5-L6),
    symbol(')',L6-L7).
    
    symbol(X,[X|Xs]-Xs).
    
    letter(X,L1-L2) :- symbol(X,L1-L2), char_type(X,alpha).


**% 4.17a ( * * ) Secuencias preordenadas e inordenadas de árboles binarios**

Consideramos árboles binarios con nodos que son identificados por
% De letras minúsculas simples.

% A) Dado un árbol binario, construye su secuencia preordenada

    preorder(T,S) :- preorder_tl(T,L), atom_chars(S,L).
    
    preorder_tl(nil,[]).
    preorder_tl(t(X,Left,Right),[X|List]) :-
    preorder_tl(Left,ListLeft),
    preorder_tl(Right,ListRight),
    append(ListLeft,ListRight,List).

    inorder(T,S) :- inorder_tl(T,L), atom_chars(S,L).
    
    inorder_tl(nil,[]).
    inorder_tl(t(X,Left,Right),List) :-
    inorder_tl(Left,ListLeft),
    inorder_tl(Right,ListRight),
    append(ListLeft,[X|ListRight],List).


**% 4.17b ( * * ) Secuencias preordenadas e inordenadas de árboles binarios**

% B) Hacer preorder / 2 e inorder / 2 reversible.
% Similar a la solución p4_17a.pl. Sin embargo, para el patrón de flujo (-, +)

% Tenemos que modificar el orden de los subobjetivos en las segundas cláusulas
% De preorder_l / 2 e inorder_l / 2

% Preorden (T, S): - S es la secuencia de preordenación transversal de la
% De nodos del árbol binario T. (árbol, átomo) (+,?) O (?, +)

> preorder(T,S) :- nonvar(T), !, preorder_tl(T,L), atom_chars(S,L). 
> preorder(T,S) :- atom(S), atom_chars(S,L), preorder_lt(T,L).

    preorder_tl(nil,[]).
    preorder_tl(t(X,Left,Right),[X|List]) :-
    preorder_tl(Left,ListLeft),
    preorder_tl(Right,ListRight),
    append(ListLeft,ListRight,List).

    preorder_lt(nil,[]).
    preorder_lt(t(X,Left,Right),[X|List]) :-
    append(ListLeft,ListRight,List),
    preorder_lt(Left,ListLeft),
    preorder_lt(Right,ListRight).

% En orden (T, S): - S es la secuencia de recorrido inorder de la
% De nodos del árbol binario T. (árbol, átomo) (+,?) O (?, +)

> inorder(T,S) :- nonvar(T), !, inorder_tl(T,L), atom_chars(S,L). 
> inorder(T,S) :- atom(S), atom_chars(S,L), inorder_lt(T,L).

    inorder_tl(nil,[]).
    inorder_tl(t(X,Left,Right),List) :-
    inorder_tl(Left,ListLeft),
    inorder_tl(Right,ListRight),
    append(ListLeft,[X|ListRight],List).

    inorder_lt(nil,[]).
    inorder_lt(t(X,Left,Right),List) :-
    append(ListLeft,[X|ListRight],List),
    inorder_lt(Left,ListLeft),
    inorder_lt(Right,ListRight).


**% 4.17c ( * * ) Secuencias preordenadas e inordenadas de árboles binarios**

% Si tanto la secuencia preordenada como la secuencia inordenada
% Los nodos de un árbol binario se dan, entonces el árbol se determina
% Sin ambigüedad.

    :- ensure_loaded(p4_17b).

% Pre_in_tree (P, I, T): - T es el árbol binario que tiene el preorden
% De secuencia P y secuencia de orden I.
% (Átomo, átomo, árbol) (+, +,?)

       pre_in_tree(P,I,T) :- preorder(T,P),     inorder(T,I).

% Esta es una buena aplicación del método generate-and-test.


% Podemos empujar el probador dentro del generador para poder
% Un (mucho) mejor rendimiento.

> pre_in_tree_push(P,I,T) :-  atom_chars(P,PL), atom_chars(I,IL),
> pre_in_tree_pu(PL,IL,T).

    pre_in_tree_pu([],[],nil).
    pre_in_tree_pu([X|PL],IL,t(X,Left,Right)) :- 
    append(ILeft,[X|IRight],IL),
    append(PLeft,PRight,PL),
    pre_in_tree_pu(PLeft,ILeft,Left),
    pre_in_tree_pu(PRight,IRight,Right).
      
% Agradable. Pero hay una solución aún mejor. Vea el problema d)!

 **4.18 ( * * ) Representación de los árboles binarios en forma de puntos < / B >**

% La sintaxis de la representación de dotstring es super simple:
%
% <Árbol> :: =. | < Letra > <árbol> <árbol>

> tree_dotstring(T,S) :- nonvar(T), !, tree_dots_dl(T,L-[]),
> atom_chars(S,L).  tree_dotstring(T,S) :- atom(S), atom_chars(S,L),
> tree_dots_dl(T,L-[]).

    tree_dots_dl(nil,L1-L2) :- symbol('.',L1-L2).
    tree_dots_dl(t(X,Left,Right),L1-L4) :- 
    letter(X,L1-L2),
    tree_dots_dl(Left,L2-L3),
    tree_dots_dl(Right,L3-L4).

> symbol(X,[X|Xs]-Xs). letter(X,L1-L2) :- symbol(X,L1-L2),
> char_type(X,alpha).









