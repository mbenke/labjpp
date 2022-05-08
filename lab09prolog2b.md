## Rekursja ogonowa

Czasami program rekurencyjny da się wykonać efektywniej jeśli rekursja jest ogonowa, np.

```
last(E, [E]).
last(E, [_|L]) :-  last(E, L).

addone([],[]).
addone([X|Xs],[Y|Ys]) :- Y is X+1, addone(Xs,Ys).
```

W drugim przypadku też mamy do czynienia z rekursją ogonową, choc odpowiednik w Haskellu nie byłby ogonowy:

```
addone [] = []
addone (x:xs) = (x+1):addone xs
```

Wynika to z faktu, ze w Prologu najpierw następuje uzgodnienie obu list w głowie klauzuli, potem uzgodnienie `Y` z wartością `X+1`, a a końcu `addone Xs Ys`.
Taką rekursję nazywamy "ogonowa modulo cons" (tail recursion modulo cons).

Spróbujmy napisać predykat `suma(L,N)`, który odnosi sukces gdy  `N` jest sumą elementów `L`

```
suma0([],0).
suma0([X|Xs], X+Y) :- suma0(Xs,Y).
```

```
?- suma0([1,2,3], N).
N = 1+(2+(3+0)).
```

nie do końca o to chodziło...niby można

```
?- suma0([1,2,3], W), N is W.
W = 1+(2+(3+0)),
N = 6.
```
ale niepotrzebnie budujemy drzewo wyrażenia. Spróbujmy inaczej:

```
suma1([],0).
suma1([X|Xs], N) :- suma1(Xs,N1), N is N1 + X.
```

```
?- suma1([1,2,3], N).
N = 6.
```

to działa, ale nie jest ogonowe ...to może zmienić kolejność?

```
zla_suma([],0).
zla_suma([X|Xs], N) :- N is N1 + X, zla_suma(Xs,N1).
```
Niestety:

```
?- zla_suma([1,2,3], N).
ERROR: Arguments are not sufficiently instantiated
```
Właściwym rozwiazaniem jest zastosowanie akumulatora: `suma2(L,A,N)` wtw gdy `N=A+suma listy L`

```
suma2([],A,A).
suma2([X|Xs],A,N) :- A1 is A+X, suma2(Xs,A1,N).
suma2(Xs,N) :- suma2(Xs,0,N).
```
NB w Haskellu pomocnicza funkcja z akumulatorem miała by inną nazwę;
w Prologu nie jest to potrzebne - `suma/2` i `suma/3` to różne predykaty.

## Napisy


Patrz też https://www.swi-prolog.org/pldoc/man?section=string

SWI Prolog ma swoiste podejście do napisów, odmienne od innych implementacji Prologu:
```
$ swipl
?- L=`kajak`.
L = [107, 97, 106, 97, 107].

?- L="kajak".
L = "kajak".

?- string_chars("kajak", X).
X = [k, a, j, a, k].
```
NB w pierwszym przykładzie odwrócone apostrofy (w zwykłych apostrofach - atomy).

Tradycyjne podejście do napisów jako list bajtów możemy odzyskać z opcją `--traditional`:

```
$ swipl --traditional

?- write("kajak").
[107,97,106,97,107]
true.

?- write(`kajak`).
ERROR: Syntax error: Operator expected
```

## Zadania

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

- rozwiazanie naiwne, bez akumulatora, wielokrotny `append` (koszt kwadratowy)
- dwa akumulatory (koszyki Dijkstry), jednokrotnie `append`
    - początkowo puste: 
    ```
    fp2(L, F) :- fp2(L, [], [], F).
    fp2([], B, C, F) :- append(B, C, F).
    ```
    - początkowo nieukonkretnione:
    
     ```
     fp3(L, F) :- fp3(L, B, C), append(B, C, F).
     ```

    - jeden akumulator (na stałe `c`), w ogóle bez `append`:
    ```
    fp4(L, F) :- fp4(L, [], F).
    ```


 i) ew. `flagaHolenderska(ListaRWB, RWB)` (flaga: red-white-blue)

 j) `quickSort(L, S)` wtw, gdy S jest wynikiem sortowania L (algorytm QuickSort)

   *  `partition(L, E, M, W)`  podzial L wzgledem E na dwie listy -  Mniejszych i Większych od E

   * wersja bez akumulatora (z append)

   * wersja z akumulatorem (czyli bez append)
   ```
   qsortA(L, S) :-  qsortA(L, [], S).
   % qsort(L, A, S) <=>  S = sort(L) ++ A  (konkatenacja)
   ```
* dla chętnych: zagwarantować logarytmiczną głebokosc stosu: partition moze dać listy różnej długości, dbamy o to aby dłuższą sortować iteracyjnie (wymaga sprytniejszego partition, które wskaże która lista jest dłuższa)

