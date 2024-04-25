---
title: Prolog 2a
---

## Jeszcze operacje na listach:

* `podlista(P, L)` wtw, gdy P jest spójną podlistą L

* Dla chętnych: `srodek(E, L)` wtw, gdy E jest środkowym elementem L
       (lista nieparzystej długości; np. `srodek(3,[1,2,3,4,5])`)

   Uwagi:
     - w tym zadaniu nie używamy jeszcze arytmetyki (nie trzeba)
     - poszukiwane rozwiązanie o koszcie liniowym.

## Arytmetyka

W Prologu `2+2` jest termem:
```
?- X = 2+2.
X = 2+2.
```

`T is U` odnosi sukces, jeśli U jest termem ustalonym reprezentującym wyrażenie arytmetyczne a T jest uzgadnialne z jego wynikiem (T może być stałą liczbową albo zmienną), np.

```
?- 2 is 1+1.
true.

?- X is 1+1.
X = 2.

?- X is 3.14*2.0.
X = 6.28.
```

Jeżeli drugi argument nie jest ustalony, dostaniemy błąd:

```
?- 2 is X+1.
ERROR: Arguments are not sufficiently instantiated
```

Ale można oczywiście

```
?- X=1, Y is X+1.
X = 1,
Y = 2.
```

Uwaga, pułapka:

```

?- 1+1 is 2.
false.
```

### Operatory infiksowe
Kanoniczna postać termu to `f(t1,t2)`; dotyczy to takze operatorów.
```
?- X is +(1,1).
X = 2.

?- X is 1+2*3.
X = 7.

?- X is +(1,*(2,3)).
X = 7.

?- is(X,1+1).
X = 2.
```

Prolog pozwala na deklaracje operatorów pre- post- oraz infiksowych. Nie ma rozróżnienia atomów alfanumerycznych i symbolicznych jak w Haskellu.

```
:- op(priorytet, rodzaj, nazwa)
```
* priorytet - liczba 1..1200 (mniejsza liczba - silniejsze wiązanie)
* rodzaj określa pozycję i wiązanie,np
   - `fx` - prefiksowy
   - `xfy` - infiksowy, wiążący w prawo
   - `yf` - postfiksowy
* nazwa - atom

Niektóre predefiniowane operatory:
```
     op(1200, xfx, ':-').
     op(1200, fx, [:-,?-]).
     op(1100, xfy,';').
     op(1050, xfy, '->')
     op(1000, xfy, ',').
     op(700, xfx, [=,is,<,>,=<,>=,==,=:=]).
     op(500, yfx, [+.-]).
     op(500, fx, [+,-,xor]).
     op(400, yfx, [*,/,div,mod]).
     op(300,xfx,mod).
```

Mozemy na przykład zdefiniować:

```
?- consult(user).
|: :- op(300,xfx,dzieli).
|:    X dzieli Y :- 0 is Y mod X.
|:    ^D% user://1 compiled 0.00 sec, 1 clauses
true.

?- 2 dzieli 4.
true.

?- 2 dzieli 5.
false.
```

Napisz predykat `enumFromTo(M, N, L)` ,który dla ustalonych `M,N` działa podobnie podobnie jak analogiczna funkcja w Haskellu, n.p.

```
    ?- enumFromTo(1,0,L).
    L = [].

    ?- enumFromTo(1,8,L).
    L = [1, 2, 3, 4, 5, 6, 7, 8].
```

Uwaga: prosze sprawdzać czy nie ma nadmiarowych odpowiedzi - błędna implementacja moze dać

```
?- enumFromTo(1,8,L).
L = [1, 2, 3, 4, 5, 6, 7, 8] ;
L = [1, 2, 3, 4, 5, 6, 7, 8, 9] ;
L = [1, 2, 3, 4, 5, 6, 7, 8, 9|...] ;
```

## Porównania

`T < U` odnosi sukces, jeśli T i U są termami ustalonymi reprezentującym wyrażenia arytmetyczne i wartość `T` jest mniejsza niż wartość `U`

```
?- 1 < 2.
true.

?- 1 < 1+1.
true.

?- 1+1 < 2+2.
true.

?- X < 1+1.
ERROR: Arguments are not sufficiently instantiated
```
Analogicznie `>, >=, =<`. Porównania dla równości wyrazeń arytmetycznych `=:=` oraz `=\=`

### Porównania bez arytmetyki

Operatory `@< @> @>= @=<`, np.

```
?- c @< b.
false.

?- c @> b.
true.
```

### Ćwiczenie
* sortowanie przez wstawianie:
```
insertionSort(+Lista, ?Posortowana),
insert(+Lista, +Elem, ?NowaLista)
```

Uwaga: znaki `+/-/?` oznaczają odpowiednio argument ukonkretniony/nieukonkretniony/dowolny; `sort(+List, -Sorted)` jest predykatem wbudowanym.


```
?- insert([],1,X).
X = [1].

?- insert([],1,[1]).
true.

?- insert([2,4],1,Z).
Z = [1, 2, 4] .

?- insert([2,4],3,Z).
Z = [2, 3, 4] .

?- insert([2,4],5,Z).
Z = [2, 4, 5].

?- insertionSort([2,1,3,7,6,5],X).
X = [1, 2, 3, 5, 6, 7] ;
false.

?- insertionSort([2],[2]).
true.

?- insertionSort([2,1],[1,2]).
true.
```
