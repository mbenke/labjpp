1. Zdefiniuj predykaty:
    * `insertBST(Elem, DrzewoOtwarte)` - wstawienie Elem do drzewa BST - drzewa otwartego
        (rozważyć możliwe zachowania gdy wstawiany element jest/nie jest w drzewie: zawsze wstawiamy, sukces bez modyfikacji drzewa gdy element juz jest)
    * `closeD(DrzewoOtwarte)`
        (zamknięcie drzewa otwartego)
    * `createBST(Lista, DrzewoBST-Zamknięte)`

    **Uwaga:** predykatów `@</2` i `@>/2` należy uzywać na termach ustalonych, inaczej można otrzymać nieoczekiwane rezultaty:
    
    ``` prolog    
    ?- X @< a.
    true.

    ?- a @< X.
    false.
    ```
    
    Dlatego przydać się tu mogą predykaty `var/1` i `nonvar/1`.
    
 **Przykłady:**
 
 ``` prolog
 ?- insertBST(b, D), insertBST(a,D), insertBST(a,D).
D = n(n(_, a, _), b, _) ;
false.

?- insertBST(b, D), insertBST(a,D), insertBST(a,D), closeD(D).
D = n(n(e, a, e), b, e) ;
false.
```

Dla uproszczenia można podobny predykat napisać dla list:

``` prolog
?- ensuremember(1,L), ensuremember(2,L), ensuremember(1,L).
L = [1, 2|_].

?- ensuremember(1,L), ensuremember(2,L), ensuremember(1,L), closeL(L).
L = [1, 2].
```
    
2. Informacje o pewnej funkcji (o skończonej dziedzinie i  przeciwdziedzinie D), są dane w postaci definicji predykatu:
    ```
        wf(x, y)  <==>  f(x) = y.
    ```
   Zdefiniuj predykat fna/? zachodzący wtw, gdy funkcja zdefiniowana
   predykatem `wf/2` jest "na".
   Dziedzina funkcji może być reprezentowana jako lista elementów.
   Czy potrafisz napisać ten program bez negacji?
   
   Zdefiniuj predykat `notfun` zachodzący wtw, gdy funkcja zdefiniowana
   predykatem `wf/2` nie jest funkcją.
   
   Zdefiniuj predykat `fun` zachodzący wtw, gdy relacja zdefiniowana
   predykatem `wf/2` jest funkcją.
   
**Uwaga:** pamiętaj, że negacji można używać tylko dla argumentów ustalonych, wpp można otrzymać niepoprawne wyniki, np.
   
``` prolog
   
?-  \+wf(X,Y).
false.

?- wf(X,Y).
X = a,
Y = 2 ;
X = a,
Y = 1.

?- \+wf(a,3).
true.
```
