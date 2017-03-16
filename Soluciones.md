


**Soluciones**
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


% A palindrome can be read forward or backward; e.g. [x,a,m,a,x]
% is_palindrome(L) :- L is a palindrome list
%    (list) (?)is_palindrome(L) :- reverse(L,L).

**% 1.07 (* *)Aplanando una estructura de una lista anidada.**

% my_flatten(L1,L2) :- the list L2 is obtained from the list L1 by
%    flattening; i.e. if an element of L1 is a list then it is replaced
%    by its elements, recursively. 
%    (list,list) (+,?)

**% Nota:** flatten(+List1, -List2) is a predefined predicate

    my_flatten(X,[X]) :- \+ is_list(X).
    my_flatten([],[]).
    my_flatten([X|Xs],Zs) :- my_flatten(X,Y), my_flatten(Xs,Ys), append(Y,Ys,Zs).


**%1.08(* *)Eliminar duplicados consecutivos de los elementos de una list.**

Si una lista contiene elementos repetidos, deberían ser reemplazados con una copia sencilla del elemnto. El orden de los elementos de no deben de ser cambiados.

% comprimir(L1,L2) :-la lista L2 es obtenida de la lista L1 
%    comprimiendo las incidencias repetidas de los elementos dentro de una copia sencilla
%    de los elementos.
%    (list,list) (+,?)

    compress([],[]).
    compress([X],[X]).
    compress([X,X|Xs],Zs) :- compress([X|Xs],Zs).
    compress([X,Y|Ys],[X|Zs]) :- X \= Y, compress([Y|Ys],Zs).

**1.09 (* *) Empaquetar los duplicados consecutivos de los elementos de una lista dentro de las sublistas.**


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

