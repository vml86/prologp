
**%  7.07 ( * * ) Sudoku**

Esta solución con listas de diferencias no es más elegante
% Solución simple con listas ordinarias (véase p7_06.pl), porque la
% De análisis se realiza quitando siempre uno o dos caracteres
% Del encabezado de la lista. Tome esto como un ejercicio fácil!

% Cada punto del rompecabezas pertenece a una fila (horizontal) y a (vertical)
% Columna, así como a un solo cuadrado 3x3 (que llamamos "cuadrado"
% para abreviar). Al principio, algunas de las manchas llevan un solo dígito
% Número entre 1 y 9. El problema es llenar los puntos faltantes con
% De dígitos de tal manera que cada número entre 1 y 9 aparece exactamente
% Una vez en cada fila, en cada columna, y en cada cuadrado.

% Representamos el rompecabezas de Sudoku como una simple lista de 81 dígitos. A
% Al principio, la lista es parcialmente instanciada. Durante el
% Proceso, todos los elememts de la lista se instancian con dígitos.

% Vamos a tratar cada spot como un spot de término Prolog (X, R, C, S) donde
% X es el número a poner en el campo, R es la fila, C la columna, y
% S el cuadrado al que pertenece el campo. R, C y S son listas que representan
% De los conjuntos de números respectivos.

% Sudoku (rompecabezas): - resolver el rompecabezas de Sudoku dado e imprimir el
% De la declaración del problema, así como la solución a la salida estándar
% (Lista de enteros, parcialmente instanciada)

s

       udoku(Puzzle) :- 
       printPuzzle(Puzzle), nl, 
       connect(Spots),
       flag(counter,_,0),
       init(Puzzle,Spots),
       solve(Spots),
       printPuzzle(Puzzle),
       flag(counter,N,N+1),
       fail.
       sudoku(_) :- 
       flag(counter,N,N), nl, 
       printCounter(N).

La parte más difícil de la solución del problema es preparar
% La lista de spot / 4 términos que representan los puntos en el rompecabezas.
Tenemos que asegurarnos de que cada punto "sabe" su fila, columna,
% y cuadrado. En otras palabras, todos los puntos en una fila
% de la misma lista para comprobar si se puede colocar un nuevo número
% en la fila. Lo mismo es cierto para las columnas y los cuadrados.

% connect(Spots) :- Spots = [spot(_,R1,C1,S1),spot(_,R1,C2,S1),.....].

    connect(Spots) :- 
    length(Spots,81),
    connectRows(Spots), 
    connectCols(Spots), 
    connectSquares(Spots).

% connectRows(Spots) :- connect the spots of all rows in the list Spot

    connectRows([]).
    connectRows(Spots) :- 
    connectRow(Spots,_,9),
    skip(Spots,9,Spots1), 
    connectRows(Spots1).

% ConnectRow (Spots, R, K): - los primeros elementos K de Spot
% Están en la misma fila R

    connectRow(_,_,0).
    connectRow([spot(_,R,_,_)|Spots],R,K) :- K > 0,
     K1 is K-1, connectRow(Spots,R,K1).

% ConnectCols (Spots): - conecta los puntos de la misma columna
connectCols(Spots) :- connectCols(Spots,9).


% connectCols(Spots,K) :- connect K more columns columns

    connectCols(_,0) :- !.
    connectCols(Spots,K) :- K > 0,
    connectCol(Spots,_),
    skip(Spots,1,Spots1), 
    K1 is K-1, connectCols(Spots1,K1).


    connectCol([],_).
    connectCol([spot(_,_,C,_)|Spots],C) :-
    skip(Spots,8,Spots1),
    connectCol(Spots1,C).

% ConnectSquares (Spots): - conecta las tres bandas
Los nueve cuadrados están dispuestos en tres bandas horizontales,
% Tres cuadrados en cada banda.

    connectSquares(Spots) :- 
    connectBand(Spots),
    skip(Spots,27,Spots1),
    connectBand(Spots1),
    skip(Spots1,27,Spots2),
    connectBand(Spots2).

    connectBand(Spots) :- 
    connectSq(Spots,_),
    skip(Spots,3,Spots1),
    connectSq(Spots1,_),
    skip(Spots1,3,Spots2),
    connectSq(Spots2,_).

% connectSq(Spots,S) :- connect the spots of square S. In the Spots
%    list each square is composed of three spot-triplets which 
%    are separated by 6 spots. 

    connectSq([],_).
    connectSq(Spots,S) :- 
    connectTriplet(Spots,S),
    skip(Spots,9,Spots1),
    connectTriplet(Spots1,S),
    skip(Spots1,9,Spots2),
    connectTriplet(Spots2,S).

% connectTriplet(Spots,S) :- Conectar los próximos tres puntos en los puntos% lista con el cuadrado S

    connectTriplet([spot(_,_,_,S),spot(_,_,_,S),spot(_,_,_,S)|_],S).

% Skip (List, N, List1): - omitir los primeros N elementos de una Lista
% Y devolver el resto de la lista en List1. Si la Lista es
% Demasiado corto, entonces tener éxito y devolver [] como resto.

    skip([],_,[]) :- !.
    skip(Xs,0,Xs) :- !.
    skip([_|Xs],K,Zs) :- K > 0, K1 is K-1, skip(Xs,K1,Zs). 


% Init (Puzzle, Spots): - inicializar la lista Spots en la
% De la sentencia del problema (Puzzle) y enlazar el
% Lista de rompecabezas a la lista de Spots

    init([],[]).
    init([X|Xs],[Sp|Spots]) :- initSpot(X,Sp), init(Xs,Spots).

% Si X no se instancia en el rompecabezas dado, cree un enlace
% Entre la variable del rompecabezas y la correspondiente
% Variable en el lugar. De lo contrario, copie el número
% Del rompecabezas en el lugar e insertarlo en el lugar
% Fila, columna y cuadrado correctos, si esto es legal.

    initSpot(X,spot(X,_,_,_)) :- var(X), !.
    initSpot(X,spot(X,R,C,S)) :- integer(X),
    insert(X,R),
    insert(X,C),
    insert(X,S).
    
% solve(Spots) :- solve the problem using backtrack

    solve([]).
    solve([Spot|Spots]) :- bind(Spot), solve(Spots).

% Blind (Spot): - enlaza el campo de datos en Spot a un
% De dígitos no conflictivos disponibles.

    bind(spot(X,_,_,_)) :- nonvar(X), !.
    bind(spot(X,R,C,S)) :- var(X),
    between(1,9,X),  % try a digit
    insert(X,R),
    insert(X,C),
    insert(X,S).

% Insert (X, L): - X puede insertarse en la lista sin
% De conflicto, es decir, X aún no está en la lista, cuando insert / 2
% se llama. De lo contrario, el predicado falla.

    insert(X,L) :- var(L), !, L = [X|_].
    insert(X,[Y|Ys]) :- X \= Y, insert(X,Ys).
    
    printPuzzle([]).
    printPuzzle(Xs) :- nl,
    printBand(Xs,Xs1),
    write('--------+---------+--------'), nl,
    printBand(Xs1,Xs2),
    write('--------+---------+--------'), nl,
    printBand(Xs2,_).
    
    printBand(Xs,Xs3) :- 
    printRow(Xs,Xs1), nl,
    write('        |         |'), nl, 
    printRow(Xs1,Xs2), nl,
    write('        |         |'), nl, 
    printRow(Xs2,Xs3), nl.

 

    printRow(Xs,Xs3) :-
    printTriplet(Xs,Xs1), write(' | '),
    printTriplet(Xs1,Xs2), write(' | '),
    printTriplet(Xs2,Xs3).

    printTriplet(Xs,Xs3) :-
    printElement(Xs,Xs1), write('  '),
    printElement(Xs1,Xs2), write('  '),
    printElement(Xs2,Xs3).

    printElement([X|Xs],Xs) :- var(X), !, write('.').
    printElement([X|Xs],Xs) :- write(X).

    printCounter(0) :- !, write('No solution'), nl.
    printCounter(1) :- !, write('1 solution'), nl.
    printCounter(K) :- write(K), write(' solutions'), 

test(N) :- puzzle(N,P), sudoku(P).

- rompecabezas(1,P) :- 
   P = [_,_,4,8,_,_,_,1,7, 6,7,_,9,_,_,_,_,_, 5,_,8,_,3,_,_,_,4,
        3,_,_,7,4,_,1,_,_, _,6,9,_,_,_,7,8,_, _,_,1,_,6,9,_,_,5,
	1,_,_,_,8,_,3,_,6, _,_,_,_,_,6,_,9,1, 2,4,_,_,_,1,5,_,_].

- rompecabezas(2,P) :- 
   P = [3,_,_,_,7,1,_,_,_, _,5,_,_,_,_,1,8,_, _,4,_,8,_,_,_,_,_,
	_,_,6,2,_,_,3,_,_, _,_,1,_,5,_,8,_,_, _,_,3,_,_,8,2,_,_,
        _,_,_,_,_,3,_,4,_, _,6,4,_,_,_,_,7,_, _,_,_,9,6,_,_,_,1].

- rompecabezas(3,P) :-
   P = [1,7,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,3,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

% un ejemplo con muchas solucione

- rompecabezas(4,P) :-
   P = [1,_,_,_,_,9,_,_,4, _,_,_,_,_,_,7,_,_, 5,_,_,3,_,_,2,_,_,
        _,8,_,_,_,_,5,_,6, _,_,_,_,8,_,_,_,_, 6,9,1,_,_,_,_,8,_,
        _,_,7,_,_,4,_,_,2, _,_,2,_,_,_,_,_,_, 3,_,_,5,_,_,_,7,1].

- rompecabezas(5,P) :- 
   P = [_,6,5,_,_,_,7,2,_, 3,_,7,_,_,_,1,_,8, 2,9,_,_,1,_,_,3,4,
        _,_,_,5,_,7,_,_,_, _,_,1,_,_,_,8,_,_, _,_,_,2,_,1,_,_,_,
        8,1,_,_,2,_,_,5,7, 7,_,2,_,_,_,9,_,1, _,5,4,_,_,_,6,8,_].

- rompecabezas(6,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% Un ejemplo con un error en la instrucción del problema (5 aparece
% Dos veces en el cuadrado superior izquierdo)

- rompecabezas(e1,P) :- 
   P = [5,_,2,_,_,3,_,_,_, 4,6,5,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].
        
% Otro ejemplo con un error en la sentencia del problema (basura
% En la primera fila

 - rompecabezas(e2,P) :- 
   P = [x,_,2,_,_,3,_,_,_, 4,6,_,_,7,_,9,_,_, _,_,3,4,_,_,_,_,_,
        9,5,_,_,6,_,_,_,_, _,4,_,_,_,_,_,9,_, _,_,_,_,9,_,_,1,7,
        _,_,_,_,_,7,2,_,_, _,_,9,_,4,_,_,3,5, _,_,_,3,_,_,7,_,6].

% Algunos ejemplos más del Sonntagszeitung

- rompecabezas(8,P) :- 
   P = [4,8,_,_,7,_,_,_,_, _,_,9,6,8,_,3,_,7, 3,_,7,4,_,_,_,5,_,
        _,_,_,3,_,_,_,2,_, 9,5,_,7,2,1,_,6,8, _,1,_,_,_,4,_,_,_,
        _,4,_,_,_,2,7,_,1, 8,_,2,_,4,7,5,_,_, _,_,_,_,5,_,_,8,4].
- rompecabezas(9,P) :- 
   P = [_,1,_,_,_,_,_,2,4, 5,_,_,_,4,_,_,8,6, 6,_,4,1,_,_,_,_,_,
        _,_,_,8,_,6,9,_,_, 8,_,_,_,_,_,_,_,2, _,_,6,4,_,3,_,_,_,
        _,_,_,_,_,7,2,_,8, 1,6,_,_,9,_,_,_,5, 7,4,_,_,_,_,_,9,_].

- rompecabezas(10,P) :-
   P = [_,9,7,_,_,5,_,_,4, _,_,_,_,_,9,_,_,_, _,_,5,_,4,_,2,_,7,
        _,8,6,_,_,3,_,_,_, _,_,_,_,2,_,_,_,_, _,_,_,5,_,_,3,4,_,
        5,_,3,_,7,_,6,_,_, _,_,_,6,_,_,_,_,_, 9,_,_,8,_,_,1,7,_].


% un rompecabezas no clasificaado "no divertido" por 
% http://dingo.sbs.arizona.edu/~sandiway/sudoku/examples.html 

- rompecabezas(11,P) :-
   P = [_,2,_,_,_,_,_,_,_, _,_,_,6,_,_,_,_,3, _,7,4,_,8,_,_,_,_,
	_,_,_,_,_,3,_,_,2, _,8,_,_,4,_,_,1,_, 6,_,_,5,_,_,_,_,_,
        _,_,_,_,1,_,7,8,_, 5,_,_,_,_,9,_,_,_, _,_,_,_,_,_,_,4,_].

% A "rompecabezas super dificil" por
% Http://www.menneske.no/sudoku/eng/showpuzzle.html?number=2155141

- rompecabezas(12,P) :-
   P = [_,_,_,6,_,_,4,_,_, 7,_,_,_,_,3,6,_,_, _,_,_,_,9,1,_,8,_,
        _,_,_,_,_,_,_,_,_, _,5,_,1,8,_,_,_,3, _,_,_,3,_,6,_,4,5,
        _,4,_,2,_,_,_,6,_, 9,_,3,_,_,_,_,_,_, _,2,_,_,_,_,1,_,_].

% some puzzles from Spektrum der Wissenschaft 3/2006, p.100

% leicht
puzzle(13,P) :-
   P = [_,2,6,4,5,8,3,_,_, 1,7,_,_,_,_,_,4,_, _,8,_,_,_,_,_,_,_,
	_,_,_,_,_,_,9,8,_, _,_,_,5,9,_,1,_,4, 7,_,_,2,_,1,_,5,_,
	_,_,_,_,4,_,_,3,_, _,_,_,8,_,_,5,_,_, 6,_,_,_,_,7,_,9,1].

% mittel
puzzle(14,P) :-
   P = [9,_,_,6,3,_,_,_,4, _,1,_,2,5,8,_,_,_, _,_,_,7,_,_,_,_,8,
        6,4,_,_,2,_,5,_,_, _,_,_,_,_,_,_,_,_, 8,2,_,5,_,_,_,9,_,
	_,_,_,_,_,_,8,7,_, 3,_,_,_,_,5,_,4,_, _,_,1,_,7,6,_,_,_].

% schwer

- rompecabezas(15,P) :-
   P = [_,_,_,_,_,_,_,_,7, _,_,_,_,_,_,6,3,4, _,_,_,9,4,_,_,2,_,
        5,_,1,7,_,_,8,6,_, _,_,9,_,_,_,_,_,3, _,_,_,_,8,_,_,_,_,
	4,3,_,5,_,_,_,_,_, _,1,_,_,6,8,_,_,_, _,_,_,_,_,3,1,_,9].

% hoellisch (!)

- rompecabezas(16,P) :-
   P = [_,_,_,_,3,_,_,_,_, _,1,5,_,_,_,6,_,_, 6,_,_,2,_,_,3,4,_,
        _,_,_,6,_,_,_,8,_, _,3,9,_,_,_,5,_,_, 5,_,_,_,_,_,9,_,2,
	_,_,_,_,_,_,_,_,_, _,_,_,9,7,_,2,5,_, 1,_,_,_,5,_,_,7,_].

% Spektrum der Wissenschaft 3/2006 Preisraetsel (angeblich hoellisch !)

- rompecabezas(17,P) :-
   P = [_,1,_,_,6,5,4,_,_, _,_,_,_,8,4,1,_,_, 4,_,_,_,_,_,_,7,_,
        _,5,_,1,9,_,_,_,_, _,_,3,_,_,_,7,_,_, _,_,_,_,3,7,_,5,_,
        _,8,_,_,_,_,_,_,3, _,_,2,6,5,_,_,_,_, _,_,9,8,1,_,_,2,_].

% La cuadrícula (casi) vacía

- rompecabezas(99,P) :- 
   P = [1,2,3,4,5,6,7,8,9, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_,
        _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_, _,_,_,_,_,_,_,_,_].
