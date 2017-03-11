


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

% 1.06 (*): Find out whether a list is a palindrome
% A palindrome can be read forward or backward; e.g. [x,a,m,a,x]
% is_palindrome(L) :- L is a palindrome list
%    (list) (?)is_palindrome(L) :- reverse(L,L).

**% 1.07 (**)Aplanando una estructura de una lista anidada.**

% my_flatten(L1,L2) :- the list L2 is obtained from the list L1 by
%    flattening; i.e. if an element of L1 is a list then it is replaced
%    by its elements, recursively. 
%    (list,list) (+,?)

**% Nota:** flatten(+List1, -List2) is a predefined predicate

    my_flatten(X,[X]) :- \+ is_list(X).
    my_flatten([],[]).
    my_flatten([X|Xs],Zs) :- my_flatten(X,Y), my_flatten(Xs,Ys), append(Y,Ys,Zs).


**%1.08(**)Eliminar duplicados consecutivos de los elementos de una list.**

Si una lista contiene elementos repetidos, deberían ser reemplazados con una copia sencilla del elemnto. El orden de los elementos de no deben de ser cambiados.

% comprimir(L1,L2) :-la lista L2 es obtenida de la lista L1 
%    comprimiendo las incidencias repetidas de los elementos dentro de una copia sencilla
%    de los elementos.
%    (list,list) (+,?)

    compress([],[]).
    compress([X],[X]).
    compress([X,X|Xs],Zs) :- compress([X|Xs],Zs).
    compress([X,Y|Ys],[X|Zs]) :- X \= Y, compress([Y|Ys],Zs).


