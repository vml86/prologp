

**Soluciones de Listas**
----------


----------

**% 5.01 Escribe un predicado istree / 1 que tiene éxito si y sólo si su argumento**
**% es un término de Prolog que representa un árbol de múltiples vías.**

% Istree (T): - T es un término que representa un árbol de múltiples vías (i), (o)
% lo siguiente es un caso de prueba:

> tree(1,t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])).
> istree(t(_,F)) :- isforest(F).
isforest([]).
isforest([T|Ts]) :- istree(T), isforest(Ts).

**% 5.02 Escriba un predicado nnodes / 2 para contar los nodos de un árbol de múltiples vías.**

% Nnodes (T, N): - el árbol multitarea T tiene N nodos (i, o))
% Lo siguiente es un caso de prueba:

> tree(1,t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])])).
>nnodes(t(_,F),N) :- nnodes(F,NF), N is NF+1.
>nnodes([],0).
nnodes([T|Ts],N) :- nnodes(T,NT), nnodes(Ts,NTs), N is NT+NTs.


% Tenga en cuenta que nnodes se llama árboles y bosques. Uno de los primeros
% Forma de polimorfismo!

% Para el patrón de flujo (o, i) podemos escribir:

    nnodes2(t(_,F),N) :- N > 0, NF is N-1, nnodes2F(F,NF).

> nnodes2F([],0).
> nnodes2F([T|Ts],N) :- N > 0, 
> between(1,N,NT), nnodes2(T,NT), 
> NTs is N-NT, nnodes2F(Ts,NTs).

**% 5.03 ( * * ) Construcción de árbol multipunto a partir de una cadena de nodos**

% Suponemos que los nodos de un árbol de múltiples vías contienen
% caracteres. En la secuencia de profundidad-primer orden de sus nodos, un
% carácter especial ^ se ha insertado siempre que, durante el
% árbol transversal, el movimiento es un retroceso al nivel anterior.

% Defina la sintaxis de la cadena y escriba un árbol de predicados (String, Tree)
% para construir el arbol cuando se da la Cadena. Trabajar con átomos (en su lugar
% de cadenas). Haga que su predicado funcione en ambas direcciones.

% Syntax in BNF:

    % < tree > ::= < letter > < forest > '^'
    
    % < forest > ::= | < tree > < forest > 

Primero una buena solución usando listas de diferencias

> tree(TS,T) :- atom(TS), !, atom_chars(TS,TL), tree_d(TL-[],T). % (+,?)
> tree(TS,T) :- nonvar(T), tree_d(TL-[],T), atom_chars(TS,TL).   % (?,+)

>tree_d([X|F1]-T, t(X,F)) :- forest_d(F1-['^'|T],F).

>forest_d(F-F,[]).
>forest_d(F1-F3,[T|F]) :- tree_d(F1-F2,T), forest_d(F2-F3,F).

Otra solución, no tan elegante como la anterior.
**% 5.03 ( * * ) Construcción de árbol multipunto a partir de una cadena de nodos**

% Suponemos que los nodos de un árbol de múltiples vías contienen
% caracteres. En la secuencia de profundidad-primer orden de sus nodos, un
% carácter especial ^ se ha insertado siempre que, durante el
% el arbol transversal, el movimiento es un retroceso al nivel anterior.

% Defina la sintaxis de la cadena y escriba un árbol de predicados (String, Tree)
% para construir el arbol cuando se da la Cadena. Trabajar con átomos (en su lugar
% de cadenas). Haga que su predicado funcione en ambas direcciones.
%

% Sintaxis en BNF:

% < Tree > :: = < letter > < forest > '^'

% < Forest > :: = | < Tree > < forest >


Primero una buena solución usando listas de diferencias

> Árbol (TS, T): - átomo (TS),!, Atom_chars (TS, TL), árbol_d (TL - [],
> T). % (+, \ Gamma)

>Árbol (TS, T): - nonvar (T), árbol_d (TL - [], T), atom_chars (TS, TL). % (\ Alpha, +)

>Tree_d ([X | F1] -T, t (X, F)): - forest_d (F1 - ['^' | T], F).

>Forest_d (F-F, []).
>Forest_d (F1-F3, [T | F]): - tree_d (F1-F2, T), forest_d (F2-F3, F).


Otra solución, no tan elegante como la anterior.

>Árbol_2 (TS, T): - átomo (TS),!, Atom_chars (TS, TL), árbol_a (TL, T). % (+, \ Gamma)
>Tree_2 (TS, T): - nonvar (T), árbol_a (TL, T), atom_chars (TS, TL). % (\ Alpha, +)

>Tree_a (TL, t (X, F)): -
Append ([X], FL, L1), append (L1, ['^'], TL), forest_a (FL, F).

>Forest_a ([], []).
Forest_a (FL, [T | Ts]): - append (TL, TsL, FL),
Tree_a (TL, T), forest_a (TsL, Ts).

**% 5,04 (*) Determina la longitud de la ruta interna de un árbol**

% Definimos la longitud de la trayectoria interna de un árbol de múltiples vías como la
% suma total de las longitudes de ruta desde la raíz a todos los nodos del árbol.
% Ipl (Árbol, L): - L es la longitud de la ruta interna del árbol Árbol
% (Árbol multitarea, entero) (+,?)

> ipl(T,L) :- ipl(T,0,L).
> 
> ipl(t(_,F),D,L) :- D1 is D+1, ipl(F,D1,LF), L is LF+D.
> 
> ipl([],_,0). ipl([T1|Ts],D,L) :- ipl(T1,D,L1), ipl(Ts,D,Ls), L is
> L1+Ls.

% Observe el polimorfismo: ipl se llama con árboles y con bosques
% como primer argumento.

**% 5.05 (*) Construya la secuencia de orden ascendente de los nodos de árbol**

% Bottom_up (Tree, Seq): - Seq es la secuencia ascendente de los nodos de
% del árbol multiruta. (+,?)

    bottom_up_f(t(X,F),Seq) :- 
    bottom_up_f(F,SeqF), append(SeqF,[X],Seq).
    
    bottom_up_f([],[]).
    bottom_up_f([T|Ts],Seq):-
    bottom_up_f(T,SeqT), bottom_up_f(Ts,SeqTs), append(SeqT,SeqTs,Seq).

% El predicado bottom_up / 2 produce un desbordamiento de pila cuando se llama
% en el patrón de flujo (-, +). Hay dos problemas con eso.
% Primero, el polimorfismo no funciona correctamente, porque durante
% que se descompone la cadena, el programa no puede adivinar si debería
% construir un árbol o un bosque a continuación. Podemos arreglar esto usando dos
% predicados separados bottom_up_tree / 2 y bottom_up_forset / 2.
En segundo lugar, si mantenemos el orden de los subobjetivos, entonces
% el intérprete cae en un bucle sin fin después de encontrar la
% primera solución. Podemos arreglar esto cambiando el orden de los
% objetivos de la siguiente manera:

    bottom_up_tree(t(X,F),Seq) :- % (?,+)
    append(SeqF,[X],Seq), bottom_up_forest(F,SeqF).
    
    bottom_up_forest([],[]).
    bottom_up_forest([T|Ts],Seq):-
    append(SeqT,SeqTs,Seq),
    bottom_up_tree(T,SeqT), bottom_up_forest(Ts,SeqTs).

% Desafortunadamente, esta versión no funciona en ambas direcciones tampoco.

Con el fin de tener un predicado que se ejecuta hacia delante y hacia atrás,
% tiene que determinar el patrón de flujo y luego llamar a uno de los anteriores
% predicados, de la siguiente manera:

    bottom_up(T,Seq) :- nonvar(T), !, bottom_up_f(T,Seq).
    bottom_up(T,Seq) :- nonvar(Seq), bottom_up_tree(T,Seq).
Esto no es muy elegante, estoy de acuerdo.

**% 5.06a ( * * ) Representación en árbol similar a Lisp**

% Esta es una solución simple para la conversión de árboles
% en "listas de fichas lispy" (i, o)
% Tree_ltl (T, L): - L es la "lista de fichas lispy" del árbol multitarea T
% (I, o)


    tree_ltl(t(X,[]),[X]).
    tree_ltl(t(X,[T|Ts]),L) :-
    tree_ltl(T,L1),
    append(['(',X],L1,L2),
    rest_ltl(Ts,L3),
    append(L2,L3,L).

    rest_ltl([],[')']).
    rest_ltl([T|Ts],L) :-
    tree_ltl(T,L1),
    rest_ltl(Ts,L2),
    append(L1,L2,L).


% Algunos predicados auxiliares

> write_ltl([]) :- nl. write_ltl([X|Xs]) :- write(X), write(' '),
> write_ltl(Xs).

> dotest(T) :- write(T), nl, tree_ltl(T,L), write_ltl(L).

> test(1) :- T = t(a,[t(b,[]),t(c,[])]), dotest(T).

> test(2) :- T = t(a,[t(b,[t(c,[])])]), dotest(T).
test(3) :- T = t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])]), dotest(T).

**% 5.06 ( * * ) Representación en árbol similar a Lisp**

% Aquí está mi solución más elegante: un solo predicado para ambos flujo
% de patrones(i, o) y (o, i)
% Tree_ltl (T, L): - L es la "lista de fichas lispy" del árbol multitarea T

    tree_ltl(T,L) :- tree_ltl_d(T,L-[]).
% Usando listas de diferencias

> tree_ltl_d(t(X,[]),[X|L]-L) :- X \= '('.
> tree_ltl_d(t(X,[T|Ts]),['(',X|L]-R) :- forest_ltl_d([T|Ts],L-[')'|R]).
> 
> forest_ltl_d([],L-L). forest_ltl_d([T|Ts],L-R) :- tree_ltl_d(T,L-M),
> forest_ltl_d(Ts,M-R).

% Algunos predicados auxiliares

> write_ltl([]) :- nl. write_ltl([X|Xs]) :- write(X), write(' '),
> write_ltl(Xs).

> dotest(T) :- write(T), nl, tree_ltl(T,L),
write_ltl(L), tree_ltl(T1,L), write(T1), nl.

> test(1) :- T = t(a,[t(b,[]),t(c,[])]), dotest(T).
test(2) :- T = t(a,[t(b,[t(c,[])])]), dotest(T).
test(3) :- T = t(a,[t(f,[t(g,[])]),t(c,[]),t(b,[t(d,[]),t(e,[])])]), 
dotest(T).

---


----------
 

