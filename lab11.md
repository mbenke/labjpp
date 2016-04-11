## Arytmetyka na liczbach wbudowanych

`T is U` odnosi sukces, jeśli U jest termem ustalonym reprezentującym wyrażenie arytmetyczne a T jest uzgadnialne z jego wynikiem (T może być stąłą liczbową albo zmienną), np.

```
| ?- 2 is 1+1.
yes
| ?- X is 1+1.
X = 2 ? ;
no
```

(no oznacza, że nie ma więcej rozwiązań niż `X = 2`)

```
| ?- X is +(1,1).
X = 2 ? ;
no
| ?- X is 1+2*3.
X = 7 ? ;
no
| ?- X is 3.14*2.0.
X = 6.28 ? ;
no
| ?- 2 is Y+1.
! Instantiation error in argument 2 of is/2
! goal:  2 is _110+1
```

`Instantiation error` bierze się stad, że `Y+1` nie jest termem ustalonym.

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
