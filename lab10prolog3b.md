Grafy
-----

(skierowane, nieskierowane, cykliczne, acykliczne, etykietowane itd.)
(reprezentacja termowa oraz reprezentacja klauzulowa)

Reprezentacja grafów: zbiory krawędzi, czyli

* listy termów postaci np. `kr(A, B)`
* klauzule unarne typu: `edge(A, B).`
* assert/1, asserta, assertz

```
?- listing(edge).
ERROR: procedure `edge' does not exist (DWIM could not correct goal)
?- assertz(edge(a,b)).
true.

?- assertz(edge(b,c)).
true.

?- listing(edge).
:- dynamic edge/2.

edge(a, b).
edge(b, c).

true.

?- retract(edge(b,c)).
true.

?- edge(X,Y).
X = a,
Y = b.
```

Przejścia miedzy reprezentacjami (na poczatku mamy zdefiniowane edge)

```
?- listing(edge/2).
edge(a, a).
edge(a, b).
edge(a, c).
edge(b, b).
edge(b, d).
edge(c, d).
edge(d, a).

?- findall(e(X,Y),edge(X,Y),G).
G = [e(a, a), e(a, b), e(a, c), e(b, b), e(b, d), e(c, d), e(d, a)].



?- [user].
|: assertkraw([]).
|: assertkraw([e(X,Y)|G]) :- assertz(kraw(X,Y)), assertkraw(G).

?- assertkraw([e(a, a), e(a, b), e(a, c), e(b, b), e(b, d), e(c, d), e(d, a)]).
true.

?- listing(kraw).
:- dynamic kraw/2.

kraw(a, a).
kraw(a, b).
kraw(a, c).
kraw(b, b).
kraw(b, d).
kraw(c, d).
kraw(d, a).
```

Usuwanie krawędzi:

```
?- retract(kraw(a,a)).
true .

?- retract(kraw(a,a)).
false.

?- retractall(kraw(X,Y)).
true.

?- listing(kraw).
:- dynamic kraw/2.

?- retractall(kraw(X,Y)).
true.

?- listing(kraw).
:- dynamic kraw/2.

?- retractall(kraw(X,Y)).
true.
```

Grafy DAG (skierowane, acykliczne)
--------------------------------

Zdefiniować predykaty:

  a) `connect(A,B)` (reprezentacja klauzulowa), `connect(Graf,A,B)` (reprezentacja termowa) wtw, gdy istnieje ścieżka z A do B.        
       Uwaga: ścieżka = niepusty (!) ciąg krawędzi

  b) `path(A,B,P)` wtw, gdy P = opis ścieżki z A do B,
                      tzn. `P = [A, ..., B]`


```
?- [acyclic].
true.

?- listing(edge).
edge(a, b).
edge(a, c).
edge(b, d).
edge(c, d).

?- connectK(a,X).
X = b ;
X = c ;
X = d ;
X = d ;
false.

?- path(a,d,P).
P = [a, b, d] ;
P = [a, c, d] ;
false.

?- [cyclic].
Warning: /home/ben/Zajecia/Jpp/Prolog/cyclic.pl:1:
Warning:    Redefined static procedure edge/2
Warning:    Previously defined at /home/ben/Zajecia/Jpp/Prolog/acyclic.pl:2
true.

?- connectK(a,X).
X = a ;
X = b ;
X = c ;
X = a ;
X = b ;
X = c .
```

Grafy (potencjalnie) cykliczne
-------------------------

Aby uniknac pętli, trzeba pamietać liste odwiedzonych wierzchołków.

  c) `pathC(A,B,P)` w dowolnym grafie skierowanym (może mieć cykle); toż w nieskierowanym

Aby uniknąć zapętlenia, trzeba pamietać liste odwiedzonych wierzchołków. W SICStus Prolog jest predykat `nonmember/2`, w SWI trzeba uzyć `\+member(...)`.

`pathC(X, Y, P) :-  pathC(X, Y, Visited, P)` ale jak zainicjować Visited?
  * `Visited=[]`
  * `Visited=[A]`
wybróbuj te sposoby i zobacz, czym się róznią.

```
?- pathC(a,d,P).
P = [a, a, b, d] ;
P = [a, a, b, b, d] ;
P = [a, a, c, d] ;
P = [a, b, d] ;
P = [a, b, b, d] ;
P = [a, c, d] ;
false.
```
ri
  d) `euler/?` - czy dany graf nieskierowany jest grafem Eulera, czyli  znalezienie (sprawdzenie) ścieżki Eulera (wprost z definicji):            ścieżka, która przechodzi przez każdą krawędź grafu dokładnie raz.

  Dla uproszczenia mozna zacząc z grafem skierowanym, ale uwaga: krawędź nieskierowana `a <-> b` to nie to samo co dwie krawędzie `a -> b, b -> a`.
  
