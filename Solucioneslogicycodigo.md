

**Soluciones Aritméticas**
----------


----------

**% 3.01 ( * * ) Tablas de verdades para expresiones lógicas.**

% Definir predicados y / 2, o / 2, nand / 2, ni / 2, xor / 2, impl / 2
% Y equ / 2 (para la equivalencia lógica) que tienen éxito o
% Fallan según el resultado de sus operaciones respectivas; p.ej.
% Y (A, B) tendrán éxito, si y sólo si ambos A y B tienen éxito.
% Tenga en cuenta que A y B pueden ser objetivos Prolog (no sólo las constantes
% True y fail).
% Una expresión lógica en dos variables se puede escribir en
% Prefix notation, como en el siguiente ejemplo: y (o (A, B), nand (A, B)).

% Ahora, escriba una tabla de predicados / 3 que imprima la tabla de verdad de un
% Dada la expresión lógica en dos variables.

**% Ejemplo:**
% \ Alpha - tabla (A, B, y (A, o (A, B))).

    True true true
    True true true

% Fail true fail
% Fail fail fail
    
    Y (A, B): - A, B.
    O (A, _): - A.
    O (_, B): - B.

    Equ (A, B): - o (y (A, B) y (no (A), no (B))).
    Xor (A, B): - no (equ (A, B)).
    Ni (A, B): - no (o (A, B)).
    Nand (A, B): - no (y (A, B)).
    Impl (A, B): - o (no (A), B).

% Bind (X): - instancia de X para ser verdadero y falso sucesivamente

> Bind (true). Enlazar (fail).
> 
> Tabla (A, B, Expr): - bind (A), bind (B), do (A, B, Expr).
> 
> (A, B, _): - escribir (A), escribir (''), escribir (B), escribir (''),
> fallar. Do (_, _, Expr): - Expr,!, Write (true), nl. Hacer (_, _, _):
> - escribir (fallar), nl.

**% 3.02 (*) Tablas de verdades para expresiones lógicas (2).**
% Continúe el problema 3.01 definiendo y / 2, o / 2, etc como
% De operadores. Esto permite escribir la expresión lógica en el
% Más natural, como en el ejemplo: A y (A o no B).
% Defina la precedencia del operador como de costumbre; Como en Java.

**% Ejemplo:**
% \ Alpha - tabla (A, B, A y (A o no B)).

    True true true
    True true true
    
% Fail true fail
% Fail fail fail
    
    : - ensure_loaded (p3_01).
    : - op (900, fy, no).
    : - op (910, yfx y).
    : - op (910, yfx, nand).
    : - op (920, yfx, o).
    : - op (920, yfx, nor).
    : - op (930, yfx, impl).
    : - op (930, yfx, equ).
    : - op (930, yfx, xor).

% I.e. No se une más fuerte que (y, nand), que se unen más fuerte que
% (O, nor) que a su vez se unen más fuerte que la implicación, la equivalencia
% Y xor.

**% 3.03 ( * * ) Tablas de verdades para expresiones lógicas (3).**

% Generalizar el problema P47 de tal manera que la lógica
% Expresión puede contener cualquier número de variables lógicas.

**% Ejemplo:**
% \ Alpha - tabla ([A, B, C], A y (B o C) equ A y B o A y C).
% True true true true

    True true true true

% True true true
% True fail fail true
% Fall true true true
% Fail true fail true
% Fall fail true true
% Fail fail fail true

    : - ensure_loaded (p3_02).

% Table (List, Expr): - imprime la tabla de verdad para la expresión Expr,
% Que contiene las variables lógicas enumeradas en List.

> Table (VarList, Expr): - bindList (VarList), do (VarList, Expr),
> falla.
> 
> BindList ([]). BindList ([V | Vs]): - bind (V), bindList (Vs).
> 
> Do (VarList, Expr): - writeVarList (VarList), writeExpr (Expr), nl.
> 
> WriteVarList ([]). WriteVarList ([V | Vs]): - escribe (V), escribe
> (''), escribeVarLista (Vs).
> 
> WriteExpr (Expr): - Expr,!, Write (true). WriteExpr (_): - escribe
> (falla).

**% ( * * ) 3.04 Códigos de gris**

% Gris (N, C): - C es el código N-bit Gray

    Gris (1, ['0', '1']).
    Gris (N, C): - N> 1, N ^ {1} es N - 1,
    Gris (N1, C1), inverso (C1, C2),
    Prepend ('0', C1, C1P),
    Prepend ('1', C2, C2P),
    Añadir (C1P, C2P, C).
    
    Prepend (_, [], []): -.
    Prepend (X, [C | Cs], [CP | CPs]): - atom_concat (X, C, CP), prepend (X, Cs, CPs).


% Esto da un buen ejemplo para la técnica de caché de resultados:

    : - gray_c / 2 dinámico.
    Gray_c (1, ['0', '1']): -.
    Gris_c (N, C): - N> 1, N _ {1} es N - 1,
    Gris_c (N1, C1), inverso (C1, C2),
    Prepend ('0', C1, C1P),
    Prepend ('1', C2, C2P),
    Añadir (C1P, C2P, C),
    Asserta ((gray_c (N, C): -)).

Intente la siguiente secuencia de objetivos y vea lo que sucede:

%? - [p3_04].
%? - listado (gray_c / 2).
% \ Alpha - gray_c (5, C).
%? - listado (gray_c / 2).


% Existe una definición alternativa para la construcción del código gris:

> Gray_alt (1, ['0', '1']). (N, C): - N> 1, N _ {1} es N - 1,   
> Gray_alt (N _ {1}, C _ {1}),    Postpend (['0', '1'], C1, C).
> 
> Posponer (_, [], []). (P, [C | Cs], [C1P, C2P | CsP]): - P = [P1, P2],
> Atom_concat (C, P1, C1P),  Atom_concat (C, P2, C2P),  Inversa (P, PR),
> Postpend (PR, Cs, CsP).

**% ( * * * ) 3.05 Código Huffman**

Suponemos que un conjunto de símbolos con sus frecuencias, dado como una lista
% De términos fr (S, F).

**% Ejemplo:** [fr (a, 45), fr (b, 13), fr (c, 12), fr (d, 16), fr (e, 9), fr (f, 5)].

% Nuestro objetivo es construir una lista hc (S, C) términos, donde C es el Huffman
% Palabra de código para el símbolo S. En nuestro ejemplo el resultado podría ser
Hc (a, '0'), hc (b, '101'), hc (c, '100'), hc (d,% Hc (f, '1100')]

% La tarea será realizada por el predicado huffman / 2 definido como sigue:
 
% Huffman (Fs, Hs): - Hs es la tabla de códigos de Huffman para la tabla de frecuencias Fs
% (Lista-de-fr / 2-términos, lista-de-hc / 2-términos) (+, -).

Durante el proceso de construcción, necesitamos nodos n (F, S) donde, al
% Comienza, F es una frecuencia y S un símbolo. Durante el proceso, como n (F, S)
% Se convierte en un nodo interno, S se convierte en un término s (L, R) con L y R siendo
% Nuevamente n (F, S). Se mantiene una lista de términos n (F, S), llamados Ns
% Como un tipo de cola de prioridad.

    Huffman (Fs, Cs): -
    Inicializar (Fs, Ns),
    Make_tree (Ns, T),
    Traverse_tree (T, Cs).

    Inicializar (Fs, Ns): - init (Fs, NsU), ordenar (NsU, Ns).
    
    en eso([],[]).
    Init ([fr (S, F) | Fs], [n (F, S) | Ns]): - init (Fs, Ns).
    
    Make_tree ([T], T).
    Make_tree ([n (F1, X1), n (F2, X2) | Ns], T): -
    F es F1 + F2,
    N (F, s (n (F1, X1), n (F2, X2))), Ns, NsR),
    Make_tree (NsR, T).

% Insert (n (F, X), Ns, NsR): - insertar el nodo n (F, X) en Ns tal que el
% NsR lista resultante se clasifica de nuevo con respecto a la frecuencia F.

    Insertar (N, [], [N]): -.
    N (F, X), [n (F0, Y) | Ns], [n (F, X), n (F0, Y) | Ns]): - F <F0,.
    (F, X), [n (F0, Y) | Ns], [n (F0, Y) | Ns1]): ).

% Traverse_tree (T, Cs): - recorrer el árbol T y construir el Huffman
% Tabla de códigos Cs,

> Traverse_tree (T, Cs): - traverse_tree (T, '', Cs1- []), ordenar (Cs1,
> Cs).
> 
> Traverse_tree (n (_, A), Código, [hc (A, Código) | Cs] -Cs): - átomo
> (A). Nodo de hojas Traverse_tree (n (_, s (Izquierda, Derecha)),
> Código, Cs1-Cs3): -% nodo interno Atom_concat (Code, '0', CodeLeft),
> Atom_concat (Código, '1', CodeRight), Traverse_tree (Izquierda,
> CodeLeft, Cs1-Cs2), Traverse_tree (Right, CodeRight, Cs2-Cs3).


% El predicado siguiente proporciona información estadística.

> Huffman (Fs): - huffman (Fs, Hs), nl, informe (Hs, 5), stats (Fs, Hs).
> 
> Informe ([], _): -!, Nl, nl. Informe (Hs, 0): -!, Nl, informe (Hs, 5).
> N], N _ {1} es N - 1, N _ {1} Writef ('% w% 8l', [S, C]), informe (Hs,
> N1).
> 
> Stats (Fs, Cs): - ordenar (Fs, FsS), ordenar (Cs, CsS), stats (FsS,
> CsS, 0,0).
> 
> Stats ([], [], FreqCodeSum, FreqSum): - El promedio es FreqCodeSum /
> FreqSum, Writef ('Longitud promedio del código (ponderado) =% w \ n',
> [Avg]). Estadística (ss), [hc (S, C) | Hs], FCS, FS): - Atom_chars (C,
> CharList), longitud (CharList, N), FCS1 es FCS + F * N, FS1 es FS + F,
> Estadísticas (Fs, Hs, FCS1, FS1).



