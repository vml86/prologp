

**Soluciones de Graficos**
----------


----------

**% 6.01 ( * * ) Conversiones** 

%entre representaciones gráficas usamos la siguiente notación:

% Adjacency-list (alist): [n (b, [c, g, h]), n (c, [b, d, f, h]), n (d, [c, f] .]
Grafico a largo plazo (gterm) ([b, c, d, f, g, h, k], [e (b, c), e (b, g), e (b, h) ]) O
(D, s), a (r, t), a (s, t), ...]

% Cláusula de borde (ecl): borde (b, g). (En la base de datos del programa)
% Arc-clause (acl): arc (r, s). (En la base de datos del programa)

(Hf): [a-b, c, g-h, d-e] o [a> b, h> g, c, b> a]

Los principales predicados de conversión son: alist_gterm / 3 y human_gterm / 2 que
% ambos (ojalá) trabajen en cualquier dirección y para gráficos, así como
% para dígrafos, etiquetados o no.

% Alist_gterm (Type, AL, GT): - convertir entre lista-adyacente y termino-grafico % de representación. El tipo es 'gráfico' o 'digrafo'.
%    (atom,alist,gterm)  (+,+,?) or (?,?,+)

>alist_gterm(Type,AL,GT):- nonvar(GT), !, gterm_to_alist(GT,Type,AL).
alist_gterm(Type,AL,GT):- atom(Type), nonvar(AL), alist_to_gterm(Type,AL,GT).

>gterm_to_alist(graph(Ns,Es),graph,AL) :- memberchk(e(_,_,_),Es), ! ,
lgt_al(Ns,Es,AL).
gterm_to_alist(graph(Ns,Es),graph,AL) :- !, 
gt_al(Ns,Es,AL).
gterm_to_alist(digraph(Ns,As),digraph,AL) :- memberchk(a(_,_,_),As), !,
ldt_al(Ns,As,AL).
gterm_to_alist(digraph(Ns,As),digraph,AL) :- 
>dt_al(Ns,As,AL).

% gráfico etiquetado
>lgt_al([],_,[]).
lgt_al([V|Vs],Es,[n(V,L)|Ns]) :-
findall(T,((member(e(X,V,I),Es) ; member(e(V,X,I),Es)),T = X/I),L),
lgt_al(Vs,Es,Ns).

% grafico sin etiqueta
>gt_al([],_,[]).
gt_al([V|Vs],Es,[n(V,L)|Ns]) :-
findall(X,(member(e(X,V),Es) ; member(e(V,X),Es)),L), gt_al(Vs,Es,Ns).


% Grafico etiquetado
>ldt_al([],_,[]).
ldt_al([V|Vs],As,[n(V,L)|Ns]) :-
findall(T,(member(a(V,X,I),As), T=X/I),L), ldt_al(Vs,As,Ns).

%Grafico sin etiqueta
>dt_al([],_,[]).
dt_al([V|Vs],As,[n(V,L)|Ns]) :-
findall(X,member(a(V,X),As),L), dt_al(Vs,As,Ns).


>alist_to_gterm(graph,AL,graph(Ns,Es)) :- !, al_gt(AL,Ns,EsU,[]), sort(EsU,Es).
>alist_to_gterm(digraph,AL,digraph(Ns,As)) :- al_dt(AL,Ns,AsU,[]), >sort(AsU,As).

>al_gt([],[],Es,Es).
al_gt([n(V,Xs)|Ns],[V|Vs],Es,Acc) :- 
add_edges(V,Xs,Acc1,Acc), al_gt(Ns,Vs,Es,Acc1). 


Agregar bordes(_,[],Es,Es).

>add_edges(V,[X/_|Xs],Es,Acc) :- V @> X, !, add_edges(V,Xs,Es,Acc).
add_edges(V,[X|Xs],Es,Acc) :- V @> X, !, add_edges(V,Xs,Es,Acc).
add_edges(V,[X/I|Xs],Es,Acc) :- V @=< X, !, add_edges(V,Xs,Es,[e(V,X,I)|Acc]).
add_edges(V,[X|Xs],Es,Acc) :- V @=< X, add_edges(V,Xs,Es,[e(V,X)|Acc]).

>al_dt([],[],As,As).
al_dt([n(V,Xs)|Ns],[V|Vs],As,Acc) :- 
 add_arcs(V,Xs,Acc1,Acc), al_dt(Ns,Vs,As,Acc1). 

>add_arcs(_,[],As,As).
add_arcs(V,[X/I|Xs],As,Acc) :- !, add_arcs(V,Xs,As,[a(V,X,I)|Acc]).
add_arcs(V,[X|Xs],As,Acc) :- add_arcs(V,Xs,As,[a(V,X)|Acc]).

% Ecl_to_gterm (GT): - construye un gráfico-término a partir de los hechos edge / 2 en el
% Base de datos del programa.

>Ecl_to_gterm (GT): -
Findall (E, (borde (X, Y), E = X-Y), Es), humano_gterm (Es, GT).

% Acl_to_gterm (GT): - construye un gráfico-término de arc / 2 hechos en el
% Base de datos del programa.

>Acl_to_gterm (GT): -
(A, (arco (X, Y), A => (X, Y)), As), human_gterm (As, GT).

% Human_gterm (HF, GT): - convertir entre human-friendly y graph-term
% de representación.
(list,gterm) (+,?) or (?,+)

> human_gterm(HF,GT):- nonvar(GT), !, gterm_to_human(GT,HF).
> human_gterm(HF,GT):- nonvar(HF), human_to_gterm(HF,GT).
> 
>gterm_to_human(graph(Ns,Es),HF) :-  memberchk(e(_,_,_),Es), !, 
lgt_hf(Ns,Es,HF).
gterm_to_human(graph(Ns,Es),HF) :-  !, 
gt_hf(Ns,Es,HF).
gterm_to_human(digraph(Ns,As),HF) :- memberchk(a(_,_,_),As), !, 
ldt_hf(Ns,As,HF).
gterm_to_human(digraph(Ns,As),HF) :- 
dt_hf(Ns,As,HF).

%Gráfico etiquetado
>lgt_hf(Ns,[],Ns).
lgt_hf(Ns,[e(X,Y,I)|Es],[X-Y/I|Hs]) :-
delete(Ns,X,Ns1),
delete(Ns1,Y,Ns2),
lgt_hf(Ns2,Es,Hs).

% Grafico sin etiqueta
>gt_hf(Ns,[],Ns).
gt_hf(Ns,[e(X,Y)|Es],[X-Y|Hs]) :-
delete(Ns,X,Ns1),
delete(Ns1,Y,Ns2),
gt_hf(Ns2,Es,Hs).

%Digrado etiquedo
>ldt_hf(Ns,[],Ns).
ldt_hf(Ns,[a(X,Y,I)|As],[X>Y/I|Hs]) :-
delete(Ns,X,Ns1),
delete(Ns1,Y,Ns2),
ldt_hf(Ns2,As,Hs).

%Digrafo no etiquetado
>dt_hf(Ns,[],Ns).
dt_hf(Ns,[a(X,Y)|As],[X>Y|Hs]) :-
delete(Ns,X,Ns1),
delete(Ns1,Y,Ns2),
dt_hf(Ns2,As,Hs).

% we guess that if there is a '>' term then it's a digraph, else a graph
human_to_gterm(HF,digraph(Ns,As)) :- memberchk(_>_,HF), !, 
hf_dt(HF,Ns1,As1), sort(Ns1,Ns), sort(As1,As).
human_to_gterm(HF,graph(Ns,Es)) :- 
hf_gt(HF,Ns1,Es1), sort(Ns1,Ns), sort(Es1,Es).
% remember: sort/2 removes duplicates!

>hf_gt([],[],[]).
hf_gt([X-Y/I|Hs],[X,Y|Ns],[e(U,V,I)|Es]) :- !, 
sort0([X,Y],[U,V]), hf_gt(Hs,Ns,Es).
hf_gt([X-Y|Hs],[X,Y|Ns],[e(U,V)|Es]) :- !,
sort0([X,Y],[U,V]), hf_gt(Hs,Ns,Es).
hf_gt([H|Hs],[H|Ns],Es) :- hf_gt(Hs,Ns,Es).

>hf_dt([],[],[]).
hf_dt([X>Y/I|Hs],[X,Y|Ns],[a(X,Y,I)|As]) :- !, 
hf_dt(Hs,Ns,As).
hf_dt([X>Y|Hs],[X,Y|Ns],[a(X,Y)|As]) :- !,
hf_dt(Hs,Ns,As).
hf_dt([H|Hs],[H

**% 6.02 (**) Ruta de acceso de un nodo a otro**

% Trayectoria (G, A, B, P): - P es una ruta (acíclica) del nodo A al nodo B en el gráfico G.
% G se presenta en forma de gráfico.

    :- ensure_loaded(p6_01).  % conversions

>path(G,A,B,P) :- path1(G,A,[B],P).
path1(_,A,[A|P1],[A|P1]).
path1(G,A,[Y|P1],P) :- 
adjacent(X,Y,G), \+ memberchk(X,[Y|P1]), path1(G,A,[X,Y|P1],P).

% Un predicado útil: adjacent / 3
>adjacent(X,Y,graph(_,Es)) :- member(e(X,Y),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(Y,X),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(X,Y,_),Es).
adjacent(X,Y,graph(_,Es)) :- member(e(Y,X,_),Es).
adjacent(X,Y,digraph(_,As)) :- member(a(X,Y),As).
adjacent(X,Y,digraph(_,As)) :- member(a(X,Y,_),As).


**% 6.03 (*) Ciclo de un nodo dado**

% Ciclo (G, A, P): - P es una trayectoria cerrada que comienza en el nodo A en el gráfico G.
% G se presenta en forma de gráfico.
%    (+,+,?)

>:- ensure_loaded(p6_01).  % conversions
:- ensure_loaded(p6_02).  % adjacent/3 and path/4

>cycle(G,A,P) :- 
adjacent(B,A,G), path(G,A,B,P1), length(P1,L), L > 2, append(P1,[A],P).

**%6.04 ( * * ) Construir todos los árboles de expansión**
>Gráfico (a, b, c, d, e, f, g, h), e (b, e), e (C, e), e (d, e), e (d, f), e (d, g), e (e, h), e (f, g), e (g, h)].

**% 6.04 ( * * ) Construir todos los árboles de expansión**

% S_tree (G, T): - T es un árbol de expansión del gráfico G
% (Gráfico-término gráfico-término) (+,?)

    :- ensure_loaded(p6_01).  % conversions

>s_tree(graph([N|Ns],GraphEdges),graph([N|Ns],TreeEdges)) :- 
transfer(Ns,GraphEdges,TreeEdgesUnsorted),  sort(TreeEdgesUnsorted,TreeEdges).

%Transferencia (Ns, GEs, TEs): - transferencia de bordes de GEs (bordes del gráfico)
% A TEs (bordes de árbol) hasta que la lista NS de nodos de árboles aún no conectados
% se convierte en vacío. Una arista se acepta si y sólo si un punto final es
% conectado al árbol y el otro no.


>transfer([],_,[]).
transfer(Ns,GEs,[GE|TEs]) :- 
select(GE,GEs,GEs1),        % modified 15-May-2001
incident(GE,X,Y),
acceptable(X,Y,Ns),
delete(Ns,X,Ns1),
delete(Ns1,Y,Ns2),
transfer(Ns2,GEs1,TEs).

>incident(e(X,Y),X,Y).
incident(e(X,Y,_),X,Y).

>acceptable(X,Y,Ns) :- memberchk(X,Ns), \+ memberchk(Y,Ns), !.
acceptable(X,Y,Ns) :- memberchk(Y,Ns), \+ memberchk(X,Ns).


% Un uso casi trivial del predicado s_tree / 2 es el siguiente
% Predictor del probador de árbol:

 % Is_tree (G): - el gráfico G es un árbol

     Is_tree (G): - s_tree (G, G),.


Otro uso es el siguiente comprobador de conectividad:
% Is_connected (G): - el gráfico G está conectado

    Is_connected (G): - s_tree (G, _),.

% Ejemplo gráfico p6_04.dat
   >test :-  
   see('p6_04.dat'), read(G), seen,
   human_gterm(H,G),
   write(H), nl, 
   setof(T,s_tree(G,T),Ts), length(Ts,N),
   write(N).

Gráfico([a, b, c, d, e, f, g, h], [e(a, b, 5), e(a, d, 3), e(b, c, 2), e(b, e, 4), e(c, e, 6), e(d, e, 7), e(d, f, 4), e(d, g, 3), e(e, h, 5), e(f, g, 4), e(g, h, **1)]).

**% 6.05 ( * * ) Construir el árbol de expansión mínimo de un gráfico etiquetado**

% Ms_tree (G, T, S): - T es un árbol de expansión mínimo del gráfico G.
% S es la suma de los valores de borde. El algoritmo de Prim.
% (Gráfico-término gráfico-término) (+,?)

>:- ensure_loaded(p6_01).  % conversions
:- ensure_loaded(p6_04).  % transfer/3, incident/3, and accept/3

>ms_tree(graph([N|Ns],GraphEdges),graph([N|Ns],TreeEdges),Sum) :- 
predsort(compare_edge_values,GraphEdges,GraphEdgesSorted),
transfer(Ns,GraphEdgesSorted,TreeEdgesUnsorted),
sort(TreeEdgesUnsorted,TreeEdges),
edge_sum(TreeEdges,Sum).

>compare_edge_values(Order,e(X1,Y1,V1),e(X2,Y2,V2)) :- compare(Order,V1+X1+Y1,V2+X2+Y2).

>edge_sum([],0).
edge_sum([e(_,_,V)|Es],S) :- edge_sum(Es,S1), S is S1 + V. 

% Ejemplo gráfico p6_05.dat

>test :-  
see('p6_05.dat'), read(G), seen,
human_gterm(H,G),
write(H), nl, 
ms_tree(G,T,S),
human_gterm(TH,T),
write(S), nl,
write(TH).
 
**% 6.06 ( * * ) Gráfico isomorfismo**

    :- ensure_loaded(p6_01).  % conversions
% Esta es una solución para gráficos solamente. No es difícil escribir el
% Predicados correspondientes para dígrafos.

% Isomórfico (G1, G2): - las gráficas G1 y G2 son isomorfas.

    isomorphic(G1,G2) :- isomorphic(G1,G2,_). 
    
% Isomórfico (G1, G2, Iso): - las gráficas G1 y G2 son isomorfas.
% Iso es una lista que representa la biyección entre el nodo
% De conjuntos de los gráficos. Es una lista abierta y contiene
% Un término i (X, Y) para cada par de nodos correspondientes

>isomorphic(graph(Ns1,Es1),graph(Ns2,Es2),Iso) :-
append(Es1,Ns1,List1),
append(Es2,Ns2,List2),
isomo(List1,List2,Iso).

% Isomo (List1, List2, Iso): - los gráficos representados por List1 y
% List2 son isomorfas.
>isomo([],[],_) :- !.
isomo([X|Xrest],Ys,Iso) :- 
select(Y,Ys,Yrest), % modified 29-11-2011
iso(X,Y,Iso),
isomo(Xrest,Yrest,Iso).

% Iso (E1, E2, Iso): - el borde E1 en un gráfico corresponde
% Al borde E2 en el otro. Tenga en cuenta que los bordes no están dirigidos.
% Iso (N1, N2, Iso): - coincide con vértices aislados.

>iso(E1,E2,Iso) :- 
   edge(E1,X1,Y1), edge(E2,X2,Y2), 
   bind(X1,X2,Iso), bind(Y1,Y2,Iso).
iso(E1,E2,Iso) :- 
   edge(E1,X1,Y1), edge(E2,X2,Y2), 
   bind(X1,Y2,Iso), bind(Y1,X2,Iso).
iso(N1,N2,Iso) :-
   \+ edge(N1,_,_),\+ edge(N2,_,_),     % isolated vertices
   bind(N1,N2,Iso).

>edge(e(X,Y),X,Y).
edge(e(X,Y,_),X,Y).


% Bind (X, Y, Iso): - es posible "enlazar X a Y" como parte del
% De bijección Iso; Es decir, un término i (X, Y) está ya en la lista Iso,
% O puede añadirse a él sin violar las reglas. Tenga en cuenta que
% Bind (X, Y, Iso) asegura que tanto X como Y son realmente "nuevos"
% Si se añade i (X, Y) a Iso.

>bind(X,Y,Iso) :- memberchk(i(X,Y0),Iso), nonvar(Y0), !, Y = Y0.
bind(X,Y,Iso) :- memberchk(i(X0,Y),Iso), X = X0.

>test(1) :-
   human_gterm([f-e,e-d,e-g,c-e,c-b,a-b,c-d,beta],G1),
   human_gterm([6-3,6-4,3-4,alfa,4-5,7-4,6-2,1-2],G2),
   isomorphic(G1,G2,Iso), write(Iso).
   
>test(2) :-
   human_gterm([f-e,e-d,e-g,c-e,c-b,a-b,c-d,beta],G1),
   human_gterm([6-3,6-4,3-4,4-5,7-4,6-2,1],G2),

>test(3) :-
   human_gterm([a-b,c-d,e,d-f],G1),
   human_gterm([1-2,1-3,5,4-6],G2),
   isomorphic(G1,G2,Iso), write(Iso).

**% 6.07 ( * * ) Grado del nodo y coloración del gráfico**

    :- ensure_loaded(p6_01).  % conversions
    :- ensure_loaded(p6_02).  % adjacent/3


% A) Escribe un grado de predicado (Gráfico, Nodo, Deg) que determina el grado
% De un nodo dado.

% Grado (Gráfico, Nodo, Grado): - Deg es el grado del nodo Nodo en el
% Gráfico Gráfico.
% (Gráfico-término, nodo, entero), (+, +,?).

>degree(graph(Ns,Es),Node,Deg) :- 
alist_gterm(graph,AList,graph(Ns,Es)),
member(n(Node,AdjList),AList), !,
length(AdjList,Deg).

% B) Escribe un predicado que genera una lista de todos los nodos de un gráfico
% Clasificados según el grado decreciente.

% Degree_sorted_nodes (Graph, Nodes): - Nodes es la lista de los nodos
% Del gráfico Gráfico, ordenado según grado decreciente.
>degree_sorted_nodes(graph(Ns,Es),DSNodes) :- 
   alist_gterm(graph,AList,graph(Ns,Es)),  
   predsort(compare_degree,AList,AListDegreeSorted),
   reduce(AListDegreeSorted,DSNodes).

>compare_degree(Order,n(N1,AL1),n(N2,AL2)) :-
 

      length(AL1,D1), length(AL2,D2),
       compare(Order,D2+N1,D1+N2).

% Nota: compare (Orden, D2 + N1, D1 + N2) ordena los nodos según
% Grado decreciente, pero en orden alfabético si los grados son iguales. ¡Genial!

    reduce([],[]).
    reduce([n(N,_)|Ns],[N|NsR]) :- reduce(Ns,NsR).

% C) Utilice el algoritmo de Welch-Powell para pintar los nodos de un gráfico de tal
%manera que los nodos adyacentes tienen colores diferentes.

% Utilice el algoritmo de Welch-Powell para pintar los nodos de un gráfico
% De tal manera que los nodos adyacentes tienen colores diferentes.

>paint(Graph,ColoredNodes) :-
   degree_sorted_nodes(Graph,DSNs),
   paint_nodes(Graph,DSNs,[],1,ColoredNodes).
   
% Paint_nodes (Graph, Ns, AccNodes, Color, ColoNodes): - pintar el resto
% Nodos Ns con un número de color Color o superior. AccNodes es el conjunto
% De nodos ya coloreados. Devuelve el resultado en ColoNodes.
% (Gráfico-término, nodo-lista, c-nodo-lista, entero, c-nodo-lista)
% (+, +, +, +, -)
>paint_nodes(_,[],ColoNodes,_,ColoNodes) :- !.
paint_nodes(Graph,Ns,AccNodes,Color,ColoNodes) :-

% Paint_nodes (Graph, DSNs, Ns, AccNodes, Color, ColoNodes): - pintar los
% nodos en Ns con un número de color fijo Color, si es posible.
% Si Ns está vacío, continúe con el siguiente número de color.
% AccNodes es el conjunto de nodos ya coloreados.
% Devuelve el resultado en ColoNodes.
% (Gráfico-término, nodo-lista, c-nodo-lista, c-nodo-lista, entero, c-nodo-lista)
%    (+,+,+,+,+,-) 
>paint_nodes(Graph,Ns,[],AccNodes,Color,ColoNodes) :- !,
   Color1 is Color+1,
   paint_nodes(Graph,Ns,AccNodes,Color1,ColoNodes).
paint_nodes(Graph,DSNs,[N|Ns],AccNodes,Color,ColoNodes) :- 
   \+ has_neighbor(Graph,N,Color,AccNodes), !,
   delete(DSNs,N,DSNs1),
   paint_nodes(Graph,DSNs1,Ns,[c(N,Color)|AccNodes],Color,ColoNodes).
paint_nodes(Graph,DSNs,[_|Ns],AccNodes,Color,ColoNodes) :- 
   paint_nodes(Graph,DSNs,Ns,AccNodes,Color,ColoNodes).
   
>has_neighbor(Graph,N,Color,AccNodes) :- 
   adjacent(N,X,Graph),
   memberchk(c(X,Color),AccNodes).

**% 6.08 ( * * ) Profundidad-primer orden de recorrido transversal (solución alternativa)**

% Escribe un predicado que genera un gráfico de profundidad-primer orden
% secuencia de recorrido. El punto de partida debe ser especificado,
% y la salida debe ser una lista de nodos que son accesibles desde
% este punto de partida (en profundidad-primer orden).

El problema principal es que si recorremos el gráfico recursivamente,
% debemos almacenar los nodos encontrados de tal manera que
% no desaparecen durante el paso de retroceso.

% En esta solución utilizamos la "base de datos registrada" que es
% más eficiente alternativa a la bien conocida afirma / retraer
% mecanismo. Consulte los manuales de SWI-Prolog para obtener más información.

% Solución alternativa usando la lista de adyacencia

    :- ensure_loaded(p6_01).  % conversions

>depth_first_order(Graph,Start,Seq) :- 
   alist_gterm(_,Alist,Graph),
   clear_rdb(dfo),
   dfo(Alist,Start),
   bagof(X,recorded(dfo,X),Seq).

>dfo(_,X) :- recorded(dfo,X).
dfo(Alist,X) :-
   \+ recorded(dfo,X),
   recordz(dfo,X),
   memberchk(n(X,AdjNodes),Alist),
   Pred =.. [dfo,Alist],        % see remark below
   checklist(Pred,AdjNodes).

>clear_rdb(Key) :-
   recorded(Key,_,Ref), erase(Ref), fail.
clear_rdb(_).

% La construcción del predicado Pred y el uso de la lista de verificación / 2
% Predicado predefinido puede parecer extraño al principio. Es equivalente a
% La siguiente construcción:
%
% Dfo (_, X): - registrado (dfo, X).
% Dfo (Alist, X): -
% \ + Registrado (dfo, X),
% Recordz (dfo, X),
% Memberchk (n (X, AdjNodes), Alist),
% Dfo_list (Alist, AdjNodes).
%
% Dfo_list (_, []).
% Dfo_list (Alist, [A | As]): - dfo (Alist, A), dfo_list (Alist, As).

% Escribe un predicado que genera un gráfico de profundidad-primer orden
% Secuencia de recorrido. El punto de partida debe ser especificado,
% Y la salida debe ser una lista de nodos que son accesibles desde
% Este punto de partida (en profundidad-primer orden).

El problema principal es que si recorremos el gráfico recursivamente,
% debemos almacenar los nodos encontrados de tal manera que
% no desaparecen durante el paso de retroceso.

% En esta solución utilizamos la "base de datos registrada" que es
% más eficiente alternativa a la bien conocida afirma / retraer
% mecanismo. Consulte los manuales de SWI-Prolog para obtener más información.

    :- ensure_loaded(p6_01).  % conversions
    :- ensure_loaded(p6_02).  % adjacent/3

>depth_first_order(Graph,Start,Seq) :- 
   (Graph = graph(Ns,_), !; Graph = digraph(Ns,_)),
   memberchk(Start,Ns),
   clear_rdb(dfo),
   recorda(dfo,Start),
   (dfo(Graph,Start); true),
   bagof(X,recorded(dfo,X),Seq).

>dfo(Graph,X) :-
   adjacent(X,Y,Graph), 
   \+ recorded(dfo,Y),
   recordz(dfo,Y),
   dfo(Graph,Y).

>clear_rdb(Key) :-
   recorded(Key,_,Ref), erase(Ref), fail.
clear_rdb(_).

**% 6.09 ( * * ) Componentes conectados (solución alternativa)**

% Escribe un predicado que divide un gráfico en sus componentes conectados.

    :- ensure_loaded(p6_01).  % conversions
    :- ensure_loaded(p6_08).  % depth_first_order/3


% Connected_components (G, Gs): - Gs es la lista de los componentes conectados
% Del gráfico G (sólo para gráficos, no para digraphs!)
% (Gterm, lista de gterms), (+, -)

>connected_components(graph([],[]),[]) :- !.
connected_components(graph(Ns,Es),[graph(Ns1,Es1)|Gs]) :-
   Ns = [N|_],
   component(graph(Ns,Es),N,graph(Ns1,Es1)),
   subtract(Ns,Ns1,NsR),
   subgraph(graph(Ns,Es),graph(NsR,EsR)),
   connected_components(graph(NsR,EsR),Gs).

>component(graph(Ns,Es),N,graph(Ns1,Es1)) :-
   depth_first_order(graph(Ns,Es),N,Seq),
   sort(Seq,Ns1),
   subgraph(graph(Ns,Es),graph(Ns1,Es1)).
   
% Subgraph (G, G1): - G1 es un subgrafo de G
>Subgrafo (grafo (Ns, Es), grafico (Ns1, Es1)): -
   Subconjunto (Ns1, Ns),
   Pred = .. [edge_is_compatible, Ns1],
   Sublista (Pred, Es, Es1).

>Edge_is_compatible (Ns1, Z): -
   (Z = e (X, Y), Z = e (X, Y, _)),
   Memberchk (X, Ns1),
   Memberchk (Y, Ns1).

% Escribe un predicado que divide un gráfico en sus componentes conectados.

    : - ensure_loaded (p6_01). % De conversiones
    : - ensure_loaded (p6_02). % Path / 4


% Connected_components (G, Gs): - Gs es la lista de los componentes conectados
% Del gráfico G (sólo para gráficos, no para digraphs!)
% (Gterm, lista de gterms), (+, -)

>connected_components(graph([],[]),[]) :- !.
connected_components(graph(Ns,Es),[graph(Ns1,Es1)|Gs]) :-
   Ns = [N|_],
   component(graph(Ns,Es),N,graph(Ns1,Es1)),
   subtract(Ns,Ns1,NsR),
   subgraph(graph(Ns,Es),graph(NsR,EsR)),
   connected_components(graph(NsR,EsR),Gs).

>component(graph(Ns,Es),N,graph(Ns1,Es1)) :-
   Pred =..[is_path,graph(Ns,Es),N],
   sublist(Pred,Ns,Ns1),
   subgraph(graph(Ns,Es),graph(Ns1,Es1)).

>is_path(Graph,A,B) :- path(Graph,A,B,_).


% Subgraph (G, G1): - G1 es un subgrafo de G
>Subgrafo (grafo (Ns, Es), grafico (Ns1, Es1)): -
   Subconjunto (Ns1, Ns),
   Pred = .. [edge_is_compatible, Ns1],
   Sublista (Pred, Es, Es1).
   
>edge_is_compatible(Ns1,Z) :- 
   (Z = e(X,Y),!; Z = e(X,Y,_)),
   memberchk(X,Ns1), 
   memberchk(Y,Ns1). 

**% 6.10 ( * * ) Gráficos bipartitos**

% Escribe un predicado que averigua si un gráfico dado es bipartito.

    :- ensure_loaded(p6_01).  % conversions
    :- ensure_loaded(p6_09).  % connected_components/2


% Is_bipartite (G): - la gráfica G es bipartita

>Is_bipartite (G): -
    Components_conectados (G, Gs),
    Lista de comprobación (is_bi, Gs).

>Is_bi (gráfico (Ns, Es)): - Ns = [N | _],
    Alist_gterm (_, Alist, graph (Ns, Es)),
    Pintura (Alist, [], rojo, N).

% Paint (Alist, ColoredNodes, Color, ActualNode)
% (+,+,+,+)

>Pintura (_, CN, color, N): -
    (C (N, Color), CN), \ alpha.
Pintura (Alist, CN, Color, N): -
    \ + Memberchk (c (N, _), CN),
    Other_color (color, otra color),
    Memberchk (n (N, AdjNodes), Alist),
    Pred = .. [pintura, Alist, [c (N, Color) | CNs], OtherColor],
    Lista de verificación (Pred, AdjNodes).

>Other_color (rojo, azul).
Other_color (azul, rojo).

**% 6.11 ( * * ) Generar gráficos simples K-regulares con N nodos.**
%
% En un gráfico K-regular todos los nodos tienen un grado de K.

% K_regular (K, N, Graph): - Graph es un gráfico K-regular simple con N nodos.
% El gráfico está en forma de gráfico. Los nodos están identificados por los números 1..N.
% Todas las soluciones pueden ser generadas a través de retroceso.
% (+, +,?) (Int, int, graph (nodos, aristas))
%
% Nota: El predicado genera la lista de Nodos y una lista de términos u (V, F)
% Que indica, para cada nodo V, el número F de los bordes no utilizados (o libres).
% Por ejemplo: con N = 5, K = 3 el algoritmo comienza con Nodos = [1,2,3,4,5]
% Y UList = [u (1,3), u (2,3), u (3,3), u (4,3), u (5,3)].
>k_regular(K,N,graph(Nodes,Edges)) :-
   range(1,N,Nodes),                         % generate Nodes list
   maplist(mku(K),Nodes,UList),              % generate initial UList
   k_reg(UList,0,Edges).
mku(K,V,u(V,K)).

% K_reg (UList, MinY, Edges): - Edges es una lista de términos e (X, Y) donde u (X, UX)
% Es el primer elemento en UList y u (Y, UY) es otro elemento de UList,
% Con Y> MinY. Tanto UX como UY, que indican el número de bordes libres
% De X e Y, respectivamente, debe ser mayor que 0. Ambos son reducidos
% Por 1 para la recursión si se elige el borde e (X, Y).
% (+, +, -) (ulist, int, elist)

>k_reg([],_,[]). 
k_reg([u(_,0)|Us],_,Edges) :- !, k_reg(Us,0,Edges).   % no more unused edges
k_reg([u(1,UX)|Us],MinY,[e(1,Y)|Edges]) :- UX > 0,    % special case X = 1
   pick(Us,Y,MinY,Us1), !,                    % pick a Y
   UX1 is UX - 1,                             % reduce number of unused edges
   k_reg([u(1,UX1)|Us1],Y,Edges).
k_reg([u(X,UX)|Us],MinY,[e(X,Y)|Edges]) :- X > 1, UX > 0,
   pick(Us,Y,MinY,Us1),                       % pick a Y
   UX1 is UX - 1,                             % reduce number of unused edges
   k_reg([u(X,UX1)|Us1],Y,Edges).

% Pick (UList_in, Y, MinY, UList_out): - hay un elemento u (Y, UY) en UList_in,
% Y es mayor que MinY y UY> 0. UList_out se obtiene de UList_in
% Reduciendo UY por 1 en el término u (Y, _). Este predicado entrega todo
% De valores posibles de Y a través de retroceso.
% (+, -, +, -) (ulist, int, int, ulist)
>pick([u(Y,UY)|Us],Y,MinY,[u(Y,UY1)|Us]) :- Y > MinY, UY > 0, UY1 is UY - 1.
pick([U|Us],Y,MinY,[U|Us1]) :- pick(Us,Y,MinY,Us1).
   
   % Rango (X, Y, Ls): - Ls es la lista de números enteros de X a Y.
% (+, +,?) (Int, int, int_list)

Rango (B, B, [B]).
(A, B, [A | L]): - A < B, A1 es A + 1, rango (A1, B, L).

   

     : - solución dinámica / 1.
     
 % All_k_regular (K, N, Gs): - Gs es la lista de todas (no isomorfas)
% K-gráficos regulares con N nodos.
% (+, +, -) (int, int, list_of_graphs)
% Nota: El predicado imprime cada nueva solución como un informe de progreso.
% Use tell ('/ dev / null') para desactivar la impresión si no le gusta.

>all_k_regular(K,N,_) :-
   retractall(solution(_)),
   k_regular(K,N,Graph),
   no_iso_solution(Graph),
   write(Graph), nl,
   assert(solution(Graph)),
   fail.
all_k_regular(_,_,Graphs) :- findall(G,solution(G),Graphs).

>:- ensure_loaded(p6_06).  % load isomorphic/2

No_iso_solution (Graph): -
   Solución (G), isomórfico (Gráfico, G),!, Falla.
No_iso_solution (_).

% El resto de este programa construye una tabla de K-regular gráficos simples
% Con N nodos para N hasta un máximo N y valores sensibles de K.
% Ejemplo:? - tabla (6).

>table(Max) :-  
   nl, write('K-regular simple graphs with N nodes'), nl,
   table(3,Max).

>table(N,Max) :- N =< Max, !,
   table(2,N,Max),
   N1 is N + 1,
   table(N1,Max).
table(_,_) :- nl. 

>table(K,N,Max) :- K < N, !,
   tell('/dev/null'),
   statistics(inferences,I1),
   all_k_regular(K,N,Gs),
   length(Gs,NSol),    
   statistics(inferences,I2),
   NInf is I2 - I1,
   told,
   plural(NSol,Pl),
   writef('\nN = %w  K = %w   %w solution%w  (%w inferences)\n',[N,K,NSol,Pl,NInf]),
   checklist(print_graph,Gs),
   K1 is K + 1,
   table(K1,N,Max).
table(_,_,_) :- nl.

>plural(X,' ') :- X < 2, !.
plural(_,'s').

>:- ensure_loaded(p6_01).  % conversion human_gterm/2

>print_graph(G) :- human_gterm(HF,G), write(HF), nl.

K-regular simple graphs with N nodes

N = 3  K = 2   1solución    (69 inferencias)
>[1-2, 1-3, 2-3]


N = 4  K = 2   1 solución   (95 inferencias)
>[1-2, 1-3, 2-4, 3-4]

N = 4  K = 3   1 solución   (124 inferencias)
>[1-2, 1-3, 1-4, 2-3, 2-4, 3-4]


N = 5  K = 2   1solución   (339 inf erencias)
>[1-2, 1-3, 2-4, 3-5, 4-5]

N = 5  K = 3   0 solución    (261 inferencias)

N = 5  K = 4   1 solución    (251 inferencias)
>[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-5, 3-4, 3-5, 4-5]


N = 6  K = 2   2 solución   (21198 inferencias)
>[1-2, 1-3, 2-3, 4-5, 4-6, 5-6]
[1-2, 1-3, 2-4, 3-5, 4-6, 5-6]

N = 6  K = 3   2 solución   (38662 inferencias)
>[1-2, 1-3, 1-4, 2-3, 2-5, 3-6, 4-5, 4-6, 5-6]
[1-2, 1-3, 1-4, 2-5, 2-6, 3-5, 3-6, 4-5, 4-6]

N = 6  K = 4   1 solución   (4698 inferencias)
>[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-5, 3-6, 4-5, 4-6, 5-6]

N = 6  K = 5   1 solución    (546 inferencias)
>[1-2, 1-3, 1-4, 1-5, 1-6, 2-3, 2-4, 2-5, 2-6, 3-4, 3-5, 3-6, 4-5, 4-6, 5-6]


N = 7  K = 2   2 solución   (150137 inferencias)
>[1-2, 1-3, 2-3, 4-5, 4-6, 5-7, 6-7]
[1-2, 1-3, 2-4, 3-5, 4-6, 5-7, 6-7]

N = 7  K = 3   0 solución   (7693 inferencias)

N = 7  K = 4   2 solutions  (4088301 inferences)
>[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-5, 3-6, 3-7, 4-6, 4-7, 5-6, 5-7, 6-7]
[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-5, 3-7, 4-6, 4-7, 5-6, 5-7, 6-7]

N = 7  K = 5   0 solution   (4849 inferencias)

N = 7  K = 6   1 solución    (1225 inferencias)
>[1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 2-3, 2-4, 2-5, 2-6, 2-7, 3-4, 3-5, 3-6, 3-7, 4-5, 
4-6, 4-7, 5-6, 5-7, 6-7]


N = 8  K = 2   3 solución   (2762047 inferencias)
>[1-2, 1-3, 2-3, 4-5, 4-6, 5-7, 6-8, 7-8]
[1-2, 1-3, 2-4, 3-4, 5-6, 5-7, 6-8, 7-8]
[1-2, 1-3, 2-4, 3-5, 4-6, 5-7, 6-8, 7-8]

N = 8  K = 3   6 solución   (67636365 inferencias)
>[1-2, 1-3, 1-4, 2-3, 2-4, 3-4, 5-6, 5-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 2-3, 2-4, 3-5, 4-6, 5-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 2-3, 2-5, 3-6, 4-5, 4-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 2-3, 2-5, 3-6, 4-7, 4-8, 5-7, 5-8, 6-7, 6-8]
[1-2, 1-3, 1-4, 2-5, 2-6, 3-5, 3-7, 4-6, 4-7, 5-8, 6-8, 7-8]
[1-2, 1-3, 1-4, 2-5, 2-6, 3-5, 3-7, 4-6, 4-8, 5-8, 6-7, 7-8]

N = 8  K = 4   6 solución   (338976076 inferencias)
>[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-5, 3-6, 3-7, 4-6, 4-8, 5-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-4, 3-7, 4-8, 5-6, 5-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-5, 3-7, 4-6, 4-8, 5-7, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-5, 3-7, 4-7, 4-8, 5-6, 5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 2-3, 2-4, 2-6, 3-7, 3-8, 4-7, 4-8, 5-6, 5-7, 5-8, 6-7, 6-8]
[1-2, 1-3, 1-4, 1-5, 2-6, 2-7, 2-8, 3-6, 3-7, 3-8, 4-6, 4-7, 4-8, 5-6, 5-7, 5-8]

>N = 8  K = 5   3solución    (259887137 inferencias)
[1-2, 1-3, 1-4, 1-5, 1-6, 2-3, 2-4, 2-5, 2-6, 3-4, 3-7, 3-8, 4-7, 4-8, 5-6, 5-7, 
5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 1-6, 2-3, 2-4, 2-5, 2-7, 3-4, 3-6, 3-8, 4-7, 4-8, 5-6, 5-7, 
5-8, 6-7, 6-8, 7-8]
[1-2, 1-3, 1-4, 1-5, 1-6, 2-3, 2-4, 2-5, 2-7, 3-6, 3-7, 3-8, 4-6, 4-7, 4-8, 5-6, 
5-7, 5-8, 6-8, 7-8]

>N = 8  K = 6   1 solución    (742954 inferencias)
[1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 2-3, 2-4, 2-5, 2-6, 2-8, 3-4, 3-5, 3-7, 3-8, 4-6, 
4-7, 4-8, 5-6, 5-7, 5-8, 6-7, 6-8, 7-8]

>N = 8  K = 7   1 solución   (2768 inferencias)
[1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 1-8, 2-3, 2-4, 2-5, 2-6, 2-7, 2-8, 3-4, 3-5, 3-6, 
3-7, 3-8, 4-5, 4-6, 4-7, 4-8, 5-6, 5-7, 5-8, 6-7, 6-8, 7-8]


    N = 9  K = 2   4 soluciones  (54627139 inferencias)
    [1-2, 1-3, 2-3, 4-5, 4-6, 5-6, 7-8, 7-9, 8-9]
    [1-2, 1-3, 2-3, 4-5, 4-6, 5-7, 6-8, 7-9, 8-9]
    [1-2, 1-3, 2-4, 3-4, 5-6, 5-7, 6-8, 7-9, 8-9]
    [1-2, 1-3, 2-4, 3-5, 4-6, 5-7, 6-8, 7-9, 8-9]