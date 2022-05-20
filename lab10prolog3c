## Obchodzenie drzewa
* `liscie(D, L)` wtw, gdy L = lista wszystkich liści, od lewej do prawej (tu może sie przydać `\+` lub `\=` do sprawdzenia, że wierzchołek nie jest liściem, inaczej mozemy dostac zbyt wiele odpowiedzi)

* `wszerz(D, L)` wtw, gdy L = lista wszystkich wierzchołków wszerz; kolejkę można na początek zrealizować przy uyciu `append`, potem zobaczymy jak mozna sprawniej.

Struktury otwarte i różnicowe
--------------------------

Pomysł. Zamiast `nil` na końcu listy postawmy tam zmienną i pamiętajmy jaka to zmienna, np
```
[1,2,3]  ~ [1,2,3|X]-X
[] ~ X-X
```

konkatenacja w czasie stałym (prześledzić przy użyciu `trace/0`):

```
dappend(X-Y,Y-Z,X-Z).     % Intuicja: X-Y + Y-Z = X-Z
```

Popatrzmy na szczególny przypadek doklejania jednego elementu:

```
?- dappend([1,2,3|Y]-Y, [E|Z]-Z,  R-Z).
Y = [E|Z],
R = [1, 2, 3, E|Z].
```

Mozemy zapisać
```
append1(X-[E|Z],E,X-Z).
% prześledź: append1([1,2,3|_X]-_X,4,Y-_Z).
```
Morał: listy różnicowe mogą mieć "ujemne" elementy - `X-[E|Z]`, to lista, której na końcu "brakuje" elementu `E`.

Podobną ideę możemy zastosować w poniższym zadaniu - próba wyjęcia elementu z kolejki "pożycza" ten element

1. Implementacja kolejki FIFO, czyli:

     a) `init(Kolejka)` - inicjalizacja kolejki (na pustą)

     b) `get(Elem, Kolejka, NowaKolejka)` - pobranie

     c) `put(Elem, Kolejka, NowaKolejka)` - wstawienie

     d) `empty(Kolejka)` - czy kolejka pusta - tu przyda się predykat `var(X)`, bo bez tego

     ```
     ?- init(Q0),get(1,Q0,Q), empty(Q0).
     Q0 = [1|_9682]-[1|_9682],
     Q = _9682-[1|_9682].
     ```
    
     Nie dla każdego termu T `T-T` reprezentuje listę pustą, tylko gdy `T jest zmienną:

     ```
     empty(Kolejka - _Koniec) :-  var(Kolejka).
     ```

     Sprawdź, co się stanie gdy pominiemy sprawdzenie `var(Kolejka)`.


2. `bfs(DrzewoBinarne, ListaWierzchWszerz)`
