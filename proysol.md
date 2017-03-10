**Listas de Prolog**
===================

**Una lista es, ya sea vacia o compuesta por un primer elemento cabeza y una cola, la cual es solo una lista por si misma**. En prolog representamos la lista vacia por un atomo [] y una lista no vacia por un termino [H|T], donde H denota head cabeza y T denota tail cola

**1.01 (*) Encuentra el último elemento de una lista.**

    > Ejemplo:
     ?- mi_ultima(X,[a,b,c,d]).
     X = d
    

----------


**1.02 (*) Encuentra el ultimo, pero un elemento de una lista.**
(de: zweitletztes Element, fr: avant-dernier élément)

> **Solución:**

>% 1.02 (*): Find the last but one element of a list

>% last_but_one(X,L) :- X is the last but one element of the list L
%    (element,list) (?,?)

>last_but_one(X,[X,_]).
last_but_one(X,[_,Y|Ys]) :- last_but_one(X,[Y|Ys]).


----------


**1.03 (*) Encuentra el elemento K'th de una lista.**
El primer elemento en la lista es el número 1.
Ejemplo:
?- element_at(X,[a,b,c,d,e],3).
X = c

> **Solución:**

> % 1.03 (*): Find the K'th element of a list.
% The first element in the list is number 1.

>% element_at(X,L,K) :- X is the K'th element of the list L
%    (element,list,integer) (?,?,+)

>% Note: nth1(?Index, ?List, ?Elem) is predefined

>element_at(X,[X|_],1).
element_at(X,[_|L],K) :- K > 1, K1 is K - 1, element_at(X,L,K1).


----------
**1.04 (*) Encuentra el numero de elementos de una lista.**

> **Solución:**

>% 1.04 (*): Find the number of elements of a list.

>% my_length(L,N) :- the list L contains N elements
%    (list,integer) (+,?) 

>% Note: length(?List, ?Int) is predefined

>my_length([],0).
my_length([_|L],N) :- my_length(L,N1), N is N1 + 1.


----------
**1.05 (*) Retroceder una lista.**

> **Solución:**
> 
> % 1.05 (*): Reverse a list.

>% my_reverse(L1,L2) :- L2 is the list obtained from L1 by reversing 
%    the order of the elements.
%    (list,list) (?,?)

>% Note: reverse(+List1, -List2) is predefined

>my_reverse(L1,L2) :- my_rev(L1,L2,[]).

>my_rev([],L2,L2) :- !.
my_rev([X|Xs],L2,Acc) :- my_rev(Xs,L2,[X|Acc]).


----------
1.06 (*) **Averigüe si una lista es un palíndromo.**
Un palíndromo puede ser leído al derecho o al revés; e.g. [x,a,m,a,x].

> **Solución:**

>% 1.06 (*): Find out whether a list is a palindrome
% A palindrome can be read forward or backward; e.g. [x,a,m,a,x]

>% is_palindrome(L) :- L is a palindrome list
%    (list) (?)

>is_palindrome(L) :- reverse(L,L).

