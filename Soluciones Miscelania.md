
**Soluciones Miscelania 7.01 - 7.06**
----------


----------

**% 7.01 ( * * ) Problema de ocho reinas**

Este es un problema clásico en informática. El objetivo es
% colocar ocho reinas en un tablero de ajedrez para que no haya dos reinas atacando
% el uno al otro; Es decir, no hay dos reinas en la misma fila, la misma columna,
% o en la misma diagonal. Generalizamos este problema original
% permitiendo una dimensión arbitraria N del tablero de ajedrez.

% Representamos las posiciones de las reinas como una lista de números 1..N.

**% Ejemplo:**
 [4,2,7,3,6,8,5,1] significa que la reina en la primera columna
% está en la fila 4, la reina en la segunda columna está en la fila 2, etc.
% Usando las permutaciones de los números 1..N garantizamos que
% no hay dos reinas en la misma fila. La única prueba que permanece
% a realizar es la prueba diagonal. Una reina colocada en la columna X y
la fila Y ocupa dos diagonales: una de ellas, con el número C = X-Y, va
% de abajo a izquierda a superior derecha, el otro, numerado D = X + Y, va
% de la parte superior izquierda a la inferior derecha. En el predicado de prueba seguimos la pista
% de las diagonales ya ocupadas en Cs y Ds.

% La primera versión es una simple solución de generación y prueba.
% Queens_1 (N, Qs): - Qs es una solución del problema de las N-reinas

> queens_1(N,Qs) :- range(1,N,Rs), permu(Rs,Qs), test(Qs).

% De rango (A, B, L): - L es la lista de números A..B

    range(A,A,[A]).
    range(A,B,[A|L]) :- A < B, A1 is A+1, range(A1,B,L).

% Permu (Xs, Zs): - la lista Zs es una permutación de la lista Xs
>permu([],[]).
>permu(Qs,[Y|Ys]) :- del(Y,Qs,Rs), permu(Rs,Ys).
>del(X,[X|Xs],Xs).
>del(X,[Y|Ys],[Y|Zs]) :- del(X,Ys,Zs).

% test(Qs) :- the list Qs represents a non-attacking queens solution

>test(Qs) :- test(Qs,1,[],[]).

% test(Qs,X,Cs,Ds) :- the queens in Qs, representing columns X to N,
% are not in conflict with the diagonals Cs and Ds

>test([],_,_,_).
test([Y|Ys],X,Cs,Ds) :- 
	C is X-Y, \+ memberchk(C,Cs),
	D is X+Y, \+ memberchk(D,Ds),
	X1 is X + 1,
	test(Ys,X1,[C|Cs],[D|Ds]).
% Ahora, en la versión 2, el probador es empujado completamente dentro del
% Generator permu.
queens_2(N,Qs) :- range(1,N,Rs), permu_test(Rs,Qs,1,[],[]).

>permu_test([],[],_,_,_).
permu_test(Qs,[Y|Ys],X,Cs,Ds) :- 
	del(Y,Qs,Rs), 
	C is X-Y, \+ memberchk(C,Cs),
	D is X+Y, \+ memberchk(D,Ds),
	X1 is X+1,
	permu_test(Rs,Ys,X1,[C|Cs],[D|Ds]).

**% 7.02 ( * * ) Tour de los Caballeros**

Otro problema famoso es este: ¿Cómo puede un caballero saltar sobre un
% NxN tablero de ajedrez de tal manera que visite cada plaza exactamente una vez?

% Knights (N, Knights): - Los Caballeros son una gira de caballeros en un 
>tablero de ajedrez NxN

>knights(N,Knights) :- M is N*N-1,  knights(N,M,[1/1],Knights).

% Closed_knights (N, Knights): - Knights es una gira de caballeros en un NxN% tablero de ajedrez que termina en la misma casilla donde comenzó.

>closed_knights(N,Knights) :- 

>   knights(N,Knights), Knights = [X/Y|_], jump(N,X/Y,1/1).

% Caballeros (N, M, Visitados, Caballeros): - la lista de cuadrados visitados debe ser
% Extendido por M cuadrados adicionales para dar la solución Caballeros de la
% NxN problema de tour de caballero de ajedrez.

    knights(_,0,Knights,Knights).
    knights(N,M,Visited,Knights) :-
       Visited = [X/Y|_],
       jump(N,X/Y,U/V),
       \+ memberchk(U/V,Visited),
       M1 is M-1,
       knights(N,M1,[U/V|Visited],Knights).

% Salta sobre un tablero de ajedrez NxN desde cuadrados A / B a C / D

    Salto (N, A / B, C / D): -
       Salto_dist (X, Y),
       C es A + X, C> 0, C = <N,
       D es B + Y, D> 0, D = <N.

% De distancias de salto

 

    Jump_dist (1,2).
    Jump_dist (2,1).
    Jump_dist (2, -1).
    Jump_dist (1, -2).
    Jump_dist (-1, -2).
    Jump_dist (-2, -1).
    Jump_dist (-2,1).
    Jump_dist (-1,2).

% Una presentación más fácil de usar de los resultados
show_knights(N) :- 
   

    get_time(Time), convert_time(Time,Tstr),
       write('Start: '), write(Tstr), nl, nl,
       knights(N,Knights), nl, show(N,Knights).

    show(N,Knights) :-
       get_time(Time), convert_time(Time,Tstr),
       write(Tstr), nl, nl,
       length(Chessboard,N),
       Pred =.. [invlength,N],
       checklist(Pred,Chessboard),
       fill_chessboard(Knights,Chessboard,1),
       checklist(show_row,Chessboard),
       nl, fail.


    invlength(N,L) :- length(L,N).
    
    show_row([]) :- nl.
    show_row([S|Ss]) :- writef('%3r',[S]), show_row(Ss). 
    
    fill_chessboard([],_,_).
    fill_chessboard([X/Y|Ks],Chessboard,K) :-
       nth1(Y,Chessboard,Row),
       nth1(X,Row,K),
       K1 is K+1,
       fill_chessboard(Ks,Chessboard,K1).



**% 7.03 ( * * * ) Conjetura de Von Koch**

Conjetura de Von Koch: Dado un árbol con N nodos (y por lo tanto N-1 bordes).
% Encontrar una manera de enumerar los nodos de 1 a n y, en consecuencia, el% de los bordes de 1 a N-1 de tal manera que para cada borde K la diferencia% de sus números de nodo es igual a K. La conjetura es que esto es siempre
% posible.

**% Ejemplo:**
 * Solución: 4 Observe que el número de nodo
% / / Diferencias de nodos adyacentes
% * - * 3 - 1 son sólo los números 1,2,3,4
% | \ | \ Que puede usarse para enumerar
% * * 2 5 los bordes.

    :- ensure_loaded(p6_01).  % conversions
    :- ensure_loaded(p6_04).  % is_tree

% Vonkoch (G, Enum): - los nodos del gráfico G pueden ser enumerados
% Como se describe en Enum. Enum es una lista de pares X / K, donde X
% Es un nodo y K el número correspondiente.

    vonkoch(Graph,Enum) :- 
       is_tree(Graph),            % check before doing too much work!
       Graph = graph(Ns,_),
       length(Ns,N),
       human_gterm(Hs,Graph),
       vonkoch(Hs,N,Enum).

> vonkoch([IsolatedNode],1,[IsolatedNode/1|_]).  % special case
> vonkoch(EdgeList,N,Enum) :-    range(1,N,NodeNumberList),     N1 is
> N-1,range(1,N1,EdgeNumberList),   
> bind(EdgeList,NodeNumberList,EdgeNumberList,Enum).

% El árbol se da como una lista de borde; p.ej. [D - a, a - g, b - c, e - f, b - e, a - b].
Nuestro problema es encontrar una biyección entre los nodos (a, b, c, ...) y
% El número entero 1..N que es compatible con la condición
% Citado anteriormente. Con el fin de construir esta biyección,
% Lista terminada Enum; Y escaneamos la lista de bordes dada.


>bind([],_,_,_) :- !.
bind([V1-V2|Es],NodeNumbers,EdgeNumbers,Enum) :-
   bind_node(V1,K1,NodeNumbers,NodeNumbers1,Enum), 
   bind_node(V2,K2,NodeNumbers1,NodeNumbers2,Enum), 
   D is abs(K1-K2), select(D,EdgeNumbers,EdgeNumbers1), % modif 15-May-2001
   bind(Es,NodeNumbers2,EdgeNumbers1,Enum).
   
% Bind_node (V, K, NodeNumsIn, NodeNumsOut, Enum): -
% V / K es un elemento de la lista Enum, y no hay V1 \ = V
% Tal que V1 / K está en Enum, y no hay K1 \ = K tal que
% V =: = K1 está en Enum. En el caso V obtiene un nuevo número, es
% Seleccionado del conjunto NodeNumsIn; Lo que queda es NodeNumsOut.
% (Nodo, entero, entero-lista, lista-entero, enumeración) (+,?, +, -,?)

>bind_node(V,K,NodeNumbers,NodeNumbers,Enum) :- 
   memberchk(V/K1,Enum), number(K1), !, K = K1.
bind_node(V,K,NodeNumbers,NodeNumbers1,Enum) :- 
   select(K,NodeNumbers,NodeNumbers1), memberchk(V/K,Enum).

% De rango (A, B, L): - L es la lista de números A..B

Rango ( B, B, [B] ): -.
(A, B, [A | L]): - A < B, A1 es A + 1, rango (A1, B, L).

% Banco de pruebas
>test(K) :-
   test_tree(K,TH),
   write(TH), nl,  
   human_gterm(TH,T),
   vonkoch(T,Enum),
   write(Enum).
      
>test_tree(1,[a-b,b-c,c-d,c-e]).
test_tree(2,[d-a,a-g,b-c,e-f,b-e,a-b]).
test_tree(3,[g-a,i-a,a-h,b-a,k-d,c-d,m-q,p-n,q-n,e-q,e-c,f-c,c-a]).
test_tree(4,[a]).


% Solución para el árbol dada en la declaración del problema:
% ?- test(3).
% [g-a, i-a, a-h, b-a, k-d, c-d, m-q, p-n, q-n, e-q, e-c, f-c, c-a]
% [a/1, b/2, c/12, g/11, h/13, i/14, d/3, e/4, f/5, k/8, q/10, m/6, n/7, p/9|_]
Observación: En la mayoría de los casos, hay muchas soluciones diferentes.

**% 7.04 ( * * * ) Rompecabezas aritmético: Dada una lista de números enteros.**

Encontrar una manera correcta de insertar signos aritméticos tales
% que el resultado sea una ecuación correcta. La idea del problema
% es de Roland Beuret. Gracias.

**% Ejemplo:**
 Con la lista de números [2, 3, 5, 7, 11] podemos formar el
% Ecuaciones 2-3 + 5 + 7 = 11 ó 2 = (3 * 5 + 7) / 11 (y otras diez).

% Ecuación (L, LT, RT): - L es la lista de números que son las hojas
% En los términos aritméticos LT y RT - de izquierda a derecha. los
% De evaluación aritmética produce el mismo resultado para LT y RT.

- equation(L,LT,RT) :-
- split(L,LL,RL),              % descomponer la lista L
- term(LL,LT),                 % construir el termino izquierdo
- term(RL,RT),                 % construir termino derecho
- LT =:= RT.                   % evaluar y comparar los terminos

% Término (L, T): - L es la lista de números que son las hojas en
% El término aritmético T - de izquierda a derecha.

- term([X],X).                    % un numero es termino por si mismo
- term([X],-X).                   % menos un
- term(L,T) :-                    % caso general: termino binario
- split(L,LL,RL),              % decomponer  lista L
- term(LL,LT),                 % construir el termino izquierdo
- term(RL,RT),                 % construir el termino derecho
- binterm(LT,RT,T).            % construir el termino binario combinado

% Binterm (LT, RT, T): - T es un término binario combinado construido a partir de
% Término a la izquierda LT y término a la derecha RT

- binterm(LT,RT,LT+RT).
- nbinterm(LT,RT,LT-RT).
- binterm(LT,RT,LT*RT).
- binterm(LT,RT,LT/RT) :- RT =\= 0.   % avoid division by zero

% Split (L, L1, L2): - dividir la lista L en partes no vacías L1 y L2
% Tal que su concatenación es L

>split(L,L1,L2) :- append(L1,L2,L), L1 = [_|_], L2 = [_|_].

% Do (L): - encontrar todas las soluciones al problema tal y como las da la lista de% números L, e imprimirlos, una solución por línea.

>do(L) :- 
   equation(L,LT,RT),
      writef('%w = %w\n',[LT,RT]),
   fail.
do(_).

Intente la siguiente meta:? - do ([2,3,5,7,11]).

**% 7.05 ( * * ) palabras numéricas en inglés**

En los documentos financieros, como los cheques, los números deben
% escribirse en palabras completas. Ejemplo: 175 debe escribirse como uno-siete-cinco.
% Escribe un predicado full_words / 1 para imprimir (no negativo) números enteros
% En palabras completas.

% Full_words (N): - imprimir el número N en palabras completas (inglés)
% (Entero no negativo) (+)


>full_words(0) :- !, write(zero), nl.
full_words(N) :- integer(N), N > 0, full_words1(N), nl.

>full_words1(0) :- !.
full_words1(N) :- N > 0,
   Q is N // 10, R is N mod 10,
   >full_words1(Q), numberword(R,RW), hyphen(Q), write(RW).

>hyphen(0) :- !.
>hyphen(Q) :- Q > 0, write('-'). 

- numberword(0,zero).
- numberword(1,one).
- numberword(2,two).
- numberword(3,three).
- numberword(4,four).
- numberword(5,five).
- numberword(6,six).
- numberword(7,seven).
- numberword(8,eight).
- numberword(9,nine).

**% 7.06 ( * * ) Comprobador de sintaxis para identificadores Ada (listas de diferencias)**

% Una sintaxis puramente recursiva es:
% < identifier > ::= < letter > < rest >

% < rest > ::=  | <optional_underscore> <letter_or_digit> < rest >

% <optional_underscore> ::=  | '_'
% <letter_or_digit> ::= < letter > | < digit >
% identifier(Str) :- Str is a legal Ada identifier
%    (atom) (+)

    identifier(S) :- atom(S), atom_chars(S,L), identifier(L-[]).
    identifier([X|L]-R) :- char_type(X,alpha), rest(L-R).

>rest(T-T) :- !.
rest(L-R) :- 
 optional_underscore(L-L1),
 letter_or_digit(L1-L2),
 rest(L2-R).

>optional_underscore(['_'|R]-R) :- !.
optional_underscore(T-T).

>letter_or_digit([X|R]-R) :- char_type(X,alpha), !.
letter_or_digit([X|R]-R) :- char_type(X,digit).


