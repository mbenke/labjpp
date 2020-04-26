## Arytmetyka na liczbach wbudowanych

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

#### Operatory infiksowe
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
* priorytet --- liczba 1..1200 (mniejsza liczba - silniejsze wiązanie)
* rodzaj określa pozycję i wiązanie,np
   - fx --- prefiksowy
   - xfy --- infiksowy, wiążący w prawo
   - yf --- postfiksowy
* nazwa --- atom

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
|    X dzieli Y :- 0 is Y mod X.
|    ^D% user://1 compiled 0.00 sec, 1 clauses
true.

?- 2 dzieli 4.
true.

?- 2 dzieli 5.
false.
```
### Porównania

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

## Technika z akumulatorem.

Zdefiniować predykaty:

 a) `suma(L, S)` wtw, gdy S = suma elementów listy L

 b) `dlugosc(L, K)` wtw, gdy K = liczba elementów listy L (`length/2`)

 c) `min(L, M)` wtw, gdy M jest minimalnym elementem L
                        (L = lista np. liczb całkowitych)

 d) `odwroc(L, R)` wtw, gdy R jest odwróconą listą L
      (np. `odwroc([1,2,3,4], [4,3,2,1])` - sukces)

 e) `palindrom(Slowo)` wtw, gdy (lista) Slowo jest palindromem
      (np. `palindrom([k,a,j,a,k])`, `palindrom([1,b,2,b,1])` - sukcesy)

 f) `slowo(Slowo)` == Slowo= $a^n b^n$      (Uwaga: bez arytmetyki!)
      dwa warianty:  (*) n > 0  (**) n >= 0
      (np. `slowo([a,a,b,b])` - sukces)

 g) slowo(Zdanie, Reszta) == Zdanie = Slowo * Reszta, Slowo - jw.
      (np. `slowo([a,a,b,b,c,d], [c,d])` - sukces)

 h) `flagaPolska(Lista, Flaga)` wtw, gdy Flaga jest posortowaną listą Lista, złożoną ze stałych b,c

      (np. `flagaPolska([b,c,b,c], [b,b,c,c])` - sukces)

 i) ew. `flagaHolenderska(ListaRWB, RWB)` (flaga: red-white-blue)

 j) `quickSort(L, S)` wtw, gdy S jest wynikiem sortowania L (algorytm QuickSort)

   * wersja bez akumulatora

   * wersja z akumulatorem (czyli bez append)

k) `flatten(L, F)` wtw, gdy L jest zagnieżdżoną listą list, których
     elementami są liczby całkowite, a F jest spłaszczoną listą L
      (np. `flatten([1,[[[[2,[3]]], 4], 5]], [1,2,3,4,5])` - sukces)
