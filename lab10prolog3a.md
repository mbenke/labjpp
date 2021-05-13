# Prolog3a - drzewa

## Drzewa binarne
Zdefiniujmy relację bycia drzewem binarnym:

```
tree(empty).
tree(node(L,_,P)) :- tree(L), tree(P).
```

jakie odpowiedzi dostaniemy na zapytanie

```
?- tree(X).
```

dlaczego?

## Dygresja

Zdefiniuj predykat `fin(+N, ?X)`, który odnosi sukces gdy X jest liczbą naturalną mniejszą od N:

```
?- fin(3,X).
X = 0 ;
X = 1 ;
X = 2 ;
false.

?- fin(0,X).
false.

?- fin(-1,X).
false.

?- findall(X,fin(4,X),L).
L = [0, 1, 2, 3].
```

`findall(Term, Goal, -Bag)` daje listę powstała z zastosowania kolejnych podstawień odpowiedzi na zapytanie `Goal` do termu `Term`

`bagof` podobnie ale odnosi porażke, jeśli zapytanie sie nie powiedzie.

`setof` podobnie, ale eliminuje duplikaty.


## Generowanie drzew

* Zdefiniuj predykat `tod(+D,?T)` - `T` jest drzewem  zrównoważonym głebokości D:

    ```
    ?- tod(2,X).
    X = node(node(empty, _14, empty, _6, node(empty, _22, empty)).
    ```

* Zdefiniuj predykat `tad(+Xs, -pT)` taki, że `T` jest drzewem, którego obejście w porządku infiksowym da listę `Xs`:

    ```
    ?- findall(T,tad([1,2,3],T), Out).
    Out = [n(e, 1, n(e, 2, n(e, 3, e))), n(e, 1, n(n(e, 2, e), 3, e)), n(n(e, 1, e), 2, n(e, 3, e)), n(n(e, 1, n(e, 2, e)), 3, e), n(n(n(e, 1, e), 2, e), 3, e)].
    ```
    (`e~empty`, `n~node`).

    Wskazówki:
    * pomocny może być predykat `split3`

    ```
    ?- findall(T,split3([1,2,3,4],T),L).
    L = [t([], 1, [2, 3, 4]), t([1], 2, [3, 4]), t([1, 2], 3, [4]), t([1, 2, 3], 4, [])].
    ```

    * w implementacji `split3` nie uzywamy `findall` etc

    * kiedy już napiszemy `split3` możemy zauważyć, że łatwo się bez niego obejść :)

* Zdefiniuj predykat `walk(+T, ?L)` taki, że `L` jest listą wierzchołków `T`  w porządku infiksowym.
    * Czy `walk` mozna uzyć zamiast `tad` do generacji drzew?
<!--    * Czy `tad` mozna uzyc do generacji wierzcholków drzewa? -->
    * Czym się róznią?

## Drzewa BST

Zdefiniować predykaty:

* `stworzBST(L, D)` wtw, gdy D jest drzewem BST zawierającym wszystkie elementy listy L (akumulator, ew. bez)

    Warto najpierw napisać `insertBST( DrzewoBST, Elem, NoweDrzewoBST )`
* `sortBST(L, S)` wtw, gdy S = lista posortowana, przy użyciu drzew BST

## Obchodzenie drzewa
* `liscie(D, L)` wtw, gdy L = lista wszystkich liści, od lewej do prawej (tu może sie przydać `\+` lub `\=` do sprawdzenia, że wierzchołek nie jest liściem, inaczej mozemy dostac zbyt wiele odpowiedzi)

* `wszerz(D, L)` wtw, gdy L = lista wszystkich wierzchołków wszerz; kolejkę mozna na razie zrealizować przyuzyciu `append`, potem zobaczymy jak mozna sprawniej.
