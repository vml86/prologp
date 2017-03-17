

**Soluciones de Listas**
----------


----------


    
**% 1.01 (*): Encuentra el último elemento de una lista.**

% My_last (X, L): - X es el último elemento de la lista L
% (Elemento, lista) (?,?)

**%Nota:** la última (? Elem,? List) está predefinida

    My_last (X, [X]).
    My_last (X, [_ | L])
    : - mi_último (X, L).


**% 1.02 (*)Encuentra el último, pero un elemento de una lista.**

% last_but_one(X,L) :- X is the last but one element of the list L
% ultimo_pero_uno(X,L) :- X es el ultimo, pero un elemento de la lista L
%    (element,list) (?,?)
%    (elemento,lista) (?,?)

    last_but_one(X,[X,_]).
    ultimo_pero_uno(X,[X,_]).
    last_but_one(X,[_,Y|Ys]) :- last_but_one(X,[Y|Ys]).
    ultimo_pero_uno(X,[_,Y|Ys]) :- ultimo_pero_uno(X,[Y|Ys]).

**%1.03 (*) Encuentra el elemento K'th de una lista.**
El primer elemento en la lista es el número 1.

% element_at(X,L,K) :- X K'th es el elmento de la lista L
%    (element,list,integer) (?,?,+)

**%Nota:** nth1(?Index, ?List, ?Elem) esta predefinido

    element_at(X,[X|_],1).
    element_at(X,[_|L],K) :- K > 1, K1 is K - 1, element_at(X,L,K1).

**%1.04 (*) Encuentra el numero de elementos de una lista.**

% mi_longitud(L,N) :- la lista L contiene N elementos
%    (list,integer) (+,?) 

**% Nota:** longitud(?List, ?Int) esta predefinida

    mi_longitud([],0).
    mi_longitud ([_|L],N) :- my_length(L,N1), N is N1 + 1.

**%1.05 (*) Retroceder una lista.**

% mi_retroceso(L1,L2) :- L2 es la lista obtenida de L1 por retroceso 
%    el orden de los elementos.
%    (list,list) (?,?)

**% Nota:** retroceder(+List1, -List2) esta predefinida

    mi_retroceso(L1,L2) :- my_rev(L1,L2,[]).
    my_rev([],L2,L2) :- !.
    my_rev([X|Xs],L2,Acc) :- my_rev(Xs,L2,[X|Acc]).

**%1.06 (*)Averigüe si una lista es un palíndromo.**
Un palíndromo puede ser leído al derecho o al revés; e.g. [x,a,m,a,x].

% 1.06 (*): Find out whether a list is a palindrome
% A palindrome can be read forward or backward; e.g. [x,a,m,a,x]
% is_palindrome(L) :- L is a palindrome list
%    (list) (?)is_palindrome(L) :- reverse(L,L).

**% 1.07 ( * * )Aplanando una estructura de una lista anidada.**

% my_flatten(L1,L2) :- the list L2 is obtained from the list L1 by
%    flattening; i.e. if an element of L1 is a list then it is replaced
%    by its elements, recursively. 
%    (list,list) (+,?)

**% Nota:** flatten(+List1, -List2) is a predefined predicate

    my_flatten(X,[X]) :- \+ is_list(X).
    my_flatten([],[]).
    my_flatten([X|Xs],Zs) :- my_flatten(X,Y), my_flatten(Xs,Ys), append(Y,Ys,Zs).


**%1.08( * * )Eliminar duplicados consecutivos de los elementos de una list.**

Si una lista contiene elementos repetidos, deberían ser reemplazados con una copia sencilla del elemnto. El orden de los elementos de no deben de ser cambiados.

% comprimir(L1,L2) :-la lista L2 es obtenida de la lista L1 
% comprimiendo las incidencias repetidas de los elementos dentro de una copia sencilla
% de los elementos.
% (list,list) (+,?)

    compress([],[]).
    compress([X],[X]).
    compress([X,X|Xs],Zs) :- compress([X|Xs],Zs).
    compress([X,Y|Ys],[X|Zs]) :- X \= Y, compress([Y|Ys],Zs).

**1.09 ( * * ) Empaquetar los duplicados consecutivos de los elementos de una lista dentro de las sublistas.**


% paquete(L1,L2) :- la lista L2 es obtenida de la lista L1, empaquetada
%    inicidencias repetidas de los elementos dentro de listas separadas.
%    (list,list) (+,?)

    pack([],[]).
    pack([X|Xs],[Z|Zs]) :- transfer(X,Xs,Ys,Z), pack(Ys,Zs).

% transferir(X,Xs,Ys,Z) Ys es la lista que permanece de la lista Xs
%    cuando todas las copias importantes de X son removidas y transferidas a Z.

    transfer(X,[],[],[X]).
    transfer(X,[Y|Ys],[Y|Ys],[X]) :- X \= Y.
    transfer(X,[X|Xs],Ys,[X|Zs]) :- transfer(X,Xs,Ys,Zs).

**% 1.10 (*)Codificación de longitud de ejecución de una lista.**

Utilice el resultado del problema 1.09 para implementar el denominado método de compresión de datos de codificación de longitud de ejecución. Los duplicados consecutivos de elementos se codifican como términos [N, E] donde N es el número de duplicados del elemento E.

% codificar(L1,L2) :- la lista L2 es obtenida de la lista L, codificando la longitud de ejecución
%    codificando. Duplicados consecutivos de los elementos que son decodificados como un terminos [N,E],
%    Donde N es el numero de duplicados de los elementos E.
%    (lista,lista) (+,?)

    :- ensure_loaded(p1_09).    
    encode(L1,L2) :- pack(L1,L), transform(L,L2).
    
    transform([],[]).
    transform([[X|Xs]|Ys],[[N,X]|Zs]) :- length([X|Xs],N),    transform(Ys,Zs).

**% 1.11 (*) Codificación de longitud de ejecución modificada**

% decodificacion_modificada (L1, L2): - la lista L2 se obtiene de la lista L1 por
% decodificación de longitud de ejecución. Los duplicados consecutivos de elementos están codificados
% como términos [N, E], donde N es el número de duplicados del elemento E.
% sin embargo, si N es igual a 1 entonces el elemento simplemente se copia en el
% lista de resultados.
% (Lista, lista) (+,?)    : - ensure_loaded (p1_10).
    
    Encode_modified (L1, L2): - encode (L1, L), Strip (L, L2).
    
    Strip([],[]).
    Strip ([[1, X] | Ys], [X | Zs]): - tira (Ys, Zs).
    [N, X] | Ys], [[N, X] | Zs]): - N> 1, tira (Ys, Zs).

**% 1.12 ( * * ) Decodificar una lista codificada de longitud de ejecución.**

% decodificar(L1,L2) :- L2  es la versión descomprimida de una lista codificada
%    de longitud de ejecución L1.
%    (lista,lista) (+,?)

    decode([],[]).
    decode([X|Ys],[X|Zs]) :- \+ is_list(X), decode(Ys,Zs).
    decode([[1,X]|Ys],[X|Zs]) :- decode(Ys,Zs).
    decode([[N,X]|Ys],[X|Zs]) :- N > 1, N1 is N - 1, decode([[N1,X]|Ys],Zs).

**% 1.13 ( * * ) Codificación de longitud de ejecución de una lista (solución directa)** 

% Encode_direct (L1, L2): - la lista L2 se obtiene de la lista L1 por
% decodificación de longitud de ejecución. Los duplicados consecutivos de elementos están codificados % Como términos [N, E], donde N es el número de duplicados del elemento E.
% Sin embargo, si N es igual a 1 entonces el elemento simplemente se copia en el
% Lista de resultados.
% (Lista, lista) (+,?)

    encode_direct([],[]).
    encode_direct([X|Xs],[Z|Zs]) :- count(X,Xs,Ys,1,Z), encode_direct(Ys,Zs).

% Count (X, Xs, Ys, K, T) Ys es la lista que queda de la lista Xs
% cuando se quitan todas las copias principales de X. T es el término [N, X],
% donde N es K más el número de X que se puede eliminar de Xs.

En el caso de N = 1, T es X, en lugar del término [1, X].

    count(X,[],[],1,X).
    count(X,[],[],N,[N,X]) :- N > 1.
    count(X,[Y|Ys],[Y|Ys],1,X) :- X \= Y.
    count(X,[Y|Ys],[Y|Ys],N,[N,X]) :- N > 1, X \= Y.
    count(X,[X|Xs],Ys,K,T) :- K1 is K + 1, count(X,Xs,Ys,K1,T).

**% 1.14 (*):Duplicar los elementos de una lista.**

% dupli(L1,L2) :- L2 es obtenido de L1  duplicando todos los elementos.
%    (lisat,lista) (?,?)

    dupli([],[]).
    dupli([X|Xs],[X,X|Ys]) :- dupli(Xs,Ys).

**% 1.15 ( * * )Duplicar los elementos de una lista un número dado de veces**

% Dupli (L1, N, L2): - L2 es obtienido de L1 duplicando todos los elementos
% N veces.
% (Lista, entero, lista) (?, +,?)

    dupli(L1,N,L2) :- dupli(L1,N,L2,N).

% Dupli (L1, N, L2, K): - L2 se obtiene de L1 duplicando su dirección
% elemento K veces, todos los demás elementos N veces.
% (Lista, entero, lista, entero) (?, +,?, +)

    dupli([],_,[],_).
    dupli([_|Xs],N,Ys,0) :- dupli(Xs,N,Ys,N).
    dupli([X|Xs],N,[X|Ys],K) :- K > 0, K1 is K - 1, dupli([X|Xs],N,Ys,K1).


**% 1.16 ( * * )Soltar cada elemento N'th de una lista**
% soltar (L1, N, L2): - L2 es obtenida de L1 soltando cada elemento N'th.
% (Lista, entero, lista) (?, +,?)
soltar (L1, N, L2): - gota (L1, N, L2, N).
% soltar (L1, N, L2, K): - L2 se obtiene de L1 copiando primeramente elementos K-1
% y luego dejar caer un elemento y, de ahí en adelante,
% N'th.
% (Lista, entero, lista, entero) (?, +,?, +)

    soltar([],_,[],_).
    soltar ([_ | Xs], N, Ys, 1): - gota (Xs, N, Ys, N).
    
    K] 1, K1 es K - 1, K _ {1}, K _ {1}
    soltar (Xs, N, Ys, K1).

**% 1.17 (*)Divida una lista en dos partes**

% De división (L, N, L1, L2): - la lista L1 contiene los primeros N elementos
% de la lista L, la lista L2 contiene los elementos restantes.
% (Lista, entero, lista, lista) (?, +,?,?)

    split(L,0,[],L).
    split([X|Xs],N,[X|Ys],Zs) :- N > 0, N1 is N - 1, split(Xs,N1,Ys,Zs).

**% 1.18 ( * * ) Extrae una porción de una lista**

% De rebanada (L1, I, K, L2): - L2 es la lista de los elementos de L1 entre
% ìndice I e índice K (ambos incluidos).
% (Lista, entero, entero, lista) (?, +, +,?)

    slice([X|_],1,1,[X]).
    slice([X|Xs],1,K,[X|Ys]) :- K > 1, 
    K1 is K - 1, slice(Xs,1,K1,Ys).
    slice([_|Xs],I,K,Ys) :- I > 1, 
    I1 is I - 1, K1 is K - 1, slice(Xs,I1,K1,Ys).

**% 1.19 ( * * ) Rotar una lista N lugares a la izquierda**

% De giro (L1, N, L2): - la lista L2 se obtiene de la lista L1 
% rotando los elementos de L1 N lugares a la izquierda.
% Ejemplos:
% Giro ([a, b, c, d, e, f, g, h], 3, [d, e, f, g, h, a, b, c]
% De rotación ([a, b, c, d, e, f, g, h], - 2, [g, h, a, b, c, d, e, f]
% (Lista, entero, lista) (+, +,?) :- ensure_loaded(p1_17).

    rotate([],_,[]) :- !.
    rotate(L1,N,L2) :-
    length(L1,NL1), N1 is N mod NL1, split(L1,N1,S1,S2), append(S2,S1,L2).

**% 1.20 (*) Quite el elemento K'th de una lista.**

% El primer elemento de la lista es el número 1.

% Remover_en (X, L, K, R): - X es el K'th elemento de la lista L; R es el
% que permanece cuando se quita el K'th elemento de L.
% (Elemento, lista, entero, lista) (?,?, +,?)

    remove_at(X,[X|Xs],1,Xs).
    remove_at(X,[Y|Xs],K,[Y|Ys]) :- K > 1, 
    K1 is K - 1, remove_at(X,Xs,K1,Ys).

**% 1.21 (*) Inserta un elemento en una posición dada en una lista** 

% El primer elemento de la lista es el número 1.

% Insert_at (X, L, K, R): - X se inserta en la lista L tal que
% ocupa la posición K. El resultado es la lista R.
% (Elemento, lista, entero, lista) (?,?, +,?):- ensure_loaded(p1_20).

    insert_at(X,L,K,R) :- remove_at(X,R,K,L).

**% 1.22(*)Crea una lista que contiene todos los enteros dentro de un rango dado**

% Rango (I, K, L): - I <= K, y L es la lista que contiene todos
% De enteros consecutivos de I a K.
% (Entero, entero, lista) (+, +,?)


**% 1.23 ( * * ) Extrae un número dado de elementos seleccionados al azar**

% De una lista.

% Rnd_select (L, N, R): - la lista R contiene N seleccionados al azar
% De artículos de la lista L.
% (Lista, entero, lista) (+, +, -):- ensure_loaded(p1_20).

    rnd_select(_,0,[]).
    rnd_select(Xs,N,[X|Zs]) :- N > 0,
    length(Xs,L),
    I is random(L) + 1,
    remove_at(X,Xs,I,Ys),
    N1 is N - 1,
    rnd_select(Ys,N1,Zs).

**% 1.24 (*):Lote: Dibuja N números aleatorios diferentes del conjunto 1..M**

% Lotto (N, M, L): - la lista L contiene N seleccionado aleatoriamente
% Números enteros del intervalo 1..M
% (Entero, entero, lista de números) (+, +, -):- ensure_loaded(p1_22).
:- ensure_loaded(p1_23).

    lotto(N,M,L) :- range(1,M,R), rnd_select(R,N,L).


**% 1.25 (*)Generar una permutación aleatoria de los elementos de una lista**

% Rnd_permu (L1, L2): - la lista L2 es una permutación aleatoria de la
% Elementos de la lista L1.
% (Lista, lista) (+, -):- ensure_loaded(p1_23).

    rnd_permu(L1,L2) :- length(L1,N),rnd_select(L1,N,L2).

**% 1.26 ( * * )Generar las combinaciones de k objetos distintos**

% Elegido de los n elementos de una lista.
% combinación (K, L, C): - C es una lista de K elementos distintos
% elegido de la lista L combinación (K, L, [X | Xs]): - K> 0,
 el (X, L, R), K1 es K-1, combinación (K1, R, Xs).
% Averigüe lo que hace exactamente el predicado el / 3.

    el(X,[X|L],L).
    el(X,[_|L],R) :- 
    el(X,L,R).

**% 1.27 ( * * ) Agrupar los elementos de un conjunto en subconjuntos disjuntos.**

% Problema a)(G, G1, G2, G3): - distribuir los 9 elementos de G en G1, G2 y G3,
% tal que G1, G2 y G3 contienen respectivamente 2,3 y 4 elementos

    Grupo3 (G, G1, G2, G3)
    Unesdoc.unesco.org unesdoc.unesco.org
    SeleccioneN (2, G, G1), Restar (G, G1, R1),
    SeleccioneN (3, R1, G2),Restar (R1, G2, R2),   
    SelectN (4, R2, G3), Restar (R2, G3, []).

% SelectN (N, L, S): - seleccionar N elementos de la lista L y ponerlos en
% el conjunto S. A través del retroceso se devuelven todas las selecciones posibles, pero
% evitar las permutaciones; Es decir, después de generar S = [a, b, c] no devuelven
% S = [b, a, c], etc.

    SeleccioneN (0, _, []): -!.
    SelectN (N, L, [X | S]): - N> 0,   
    El (X, L, R),
    N _ {1} es N - 1,
    SeleccioneN (N1, R, S).

% Utilice el / 3 de p1_26:: - ensure_loaded (p1_26).
% El (X, [X | L], L).
% El (X, [_ | L], R): - el (X, L, R).
% Subtract / 3 está predefinido
% Problema b): Generalización
% Grupo (G, Ns, Gs): - distribuir los elementos de G en los grupos Gs.
% Los tamaños de grupo se dan en la lista Ns.

    group([],[],[]).
    group(G,[N1|Ns],[G1|Gs]) :- 
    selectN(N1,G,G1),
    subtract(G,G1,R),
    group(R,Ns,Gs).

**% 1.28 ( * * ) Clasificación de una lista de listas según la longitud**

% A) tipo de longitud
% Lsort (InList, OutList): - se supone que los elementos de InList
% son listas. Entonces OutList se obtiene de InList mediante la clasificación
% de sus elementos según su longitud. Lsort / 2 ordena ascendentemente,
% Lsort / 3 permite tipos ascendentes o descendentes.
% (List_of_lists, list_of_lists), (+,?)Lsort (InList, OutList): - lsort (InList, OutList, asc).
% Dirección de clasificación Dir es asc o desc

    lsort(InList,OutList,Dir) :-   add_key(InList,KList,Dir),   keysort(KList,SKList),   rem_key(SKList,OutList).
    add_key([],[],_).
    add_key([X|Xs],[L-p(X)|Ys],asc) :- !, 	length(X,L), add_key(Xs,Ys,asc).
    add_key([X|Xs],[L-p(X)|Ys],desc) :- 	length(X,L1), L is -L1, add_key(Xs,Ys,desc).
    rem_key([],[]).
    rem_key([_-p(X)|Xs],[X|Ys]) :- rem_key(Xs,Ys).

% B) clasificación de frecuencia de longitud
%% Lfsort (InList, OutList): - Se supone que los elementos de InList
% son listas. Entonces OutList se obtiene de InList mediante la clasificación
% de sus elementos según su frecuencia de longitud; Es decir, en el predeterminado,
% donde la clasificación se realiza de forma ascendente, se colocan listas con longitudes raras
% primero, otros con longitudes más frecuentes vienen más tarde.

**% Ejemplo:**
,,,,,,,,,,,,,,,,,,,,,,,,,, [O]], L).

Ld {e}, [d, e], [d, e], [a, b, c], [f, g, h] ]]

%% tome en cuenta que las dos primeras listas en el Resultado tienen longitud 4 y 1, ambas
% longitudes aparecen sólo una vez. La tercera y cuarta lista tienen longitud 3 que
% aparecen, hay dos lista de esta longitud. Y finalmente, las ultimas
% tres listas tienen longitud 2.
 Esta es la longitud más frecuente.

    Lfsort (InList, OutList): - lfsort (InList, OutList, asc).

% Dirección de clasificación Dir es asc o desc

    lfsort(InList,OutList,Dir):-add_key(InList,KList,desc),   keysort(KList,SKList),
    pac(SKList,PKList),   lsort(PKList,SPKList,Dir),   flatten(SPKList,FKList),
    rem_key(FKList,OutList).  

    pac([],[]).pac([L-X|Xs],[[L-X|Z]|Zs]) :- transf(L-X,Xs,Ys,Z), pac(Ys,Zs).

% Transf (L-X, Xs, Ys, Z) Ys es la lista que queda de la lista Xs
% cuando todas las copias principales de longitud L se eliminan y se transfieren a Z

    transf(_,[],[],[]).transf(L-_,[K-Y|Ys],[K-Y|Ys],[]) :- L \= K.
    transf(L-_,[L-X|Xs],Ys,[L-X|Zs]) :- transf(L-X,Xs,Ys,Zs).
    test :- L = [[a,b,c],[d,e],[f,g,h],[d,e],[i,j,k,l],[m,n],[o]],  
    write('L = '), write(L), nl,
    lsort(L,LS),
    write('LS = '), write(LS), nl,
    lsort(L,LSD,desc),
    write('LSD = '), write(LSD), nl,
    lfsort(L,LFS),
    write('LFS = '), write(LFS), nl.


---


----------
