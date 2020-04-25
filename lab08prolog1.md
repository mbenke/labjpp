1. Dana relacja:  dziecko(Dziecko, Matka, Ojciec).

 Na przykład:

    ~~~~
    % dziecko(Dziecko, Matka, Ojciec)
    dziecko(jasio, ewa, jan).
    dziecko(stasio, ewa, jan).
    dziecko(basia, anna, piotr).
    dziecko(jan, ela, jakub).
    ~~~~

 Zdefiniować predykaty:

 a) `ojciec(Dziecko, Ojciec)`

 b) `matka(Dziecko, Matka)`

 c) `rodzic(Dziecko, Rodzic)`

 d) `babcia(Dziecko, Babcia)` (ew. dziadek)

 e) `wnuk(Wnuk, Babcia/Dziadek)`

 f) `przodek(Przodek, Potomek)`

2. Rozszerzenie danych o dzieciach, np.:

    ~~~
      dziecko(Dziecko, Matka, Ojciec, Płeć),
    ~~~

   gdzie `Płeć = ch` (chłopiec) lub `dz` (dziewczynka)

    Zdefiniować predykaty:
     
     a) `syn(Dziecko, Matka, Ojciec)`

     b) `corka(Dziecko, Matka, Ojciec)`

     c) `dziecko(Dziecko, Matka, Ojciec)`

     d) `wnuczka(Dziecko, Babcia/Dziadek)`
        lub `wnuk(Dziecko, Babcia/Dziadek)`

3. Operacje na liczbach naturalnych (0, s/1)
    Zdefiniować predykaty:

     a) `nat(x)` wtw, gdy $x$ jest liczbą naturalną

     b) `plus(x, y, z)` wtw, gdy $x + y = z$

     c) `minus(x, y, z)` wtw, gdy $x - y = z$

     d) `fib(k, n)` wtw, gdy $n = k$-ta liczba Fibonacciego


4. Proste relacje na listach.
   Zdefiniować predykaty:

    a) `lista(L)` wtw, gdy L jest (prologową) listą

    b) `pierwszy(E, L)` wtw, gdy E jest pierwszym elementem L

    c) `ostatni(E, L)` wtw, gdy E jest ostatnim elementem L

    d) `element(E, L)` wtw, gdy E jest (dowolnym) elementem L
       (czyli `member/2`)

    e) `srodek(E, L)` wtw, gdy E jest środkowym elementem L
       (lista nieparzystej długości; np. `srodek(3,[1,2,3,4,5])`)

    f) `podziel(Lista, NieParz, Parz)` == podział danej listy na dwie
       podlisty zawierające odpowiednio kolejne elementy z parzystych
       (nieparzystych) pozycji

    g) `scal(L1, L2, L3)` wtw, gdy L3 = konkatenacja listy L1 z L2
       (czyli append/3),

    h) `podlista(P, L)` wtw, gdy P jest spójną podlistą L

    i) `podciag(P, L)`  wtw, gdy P jest podciągiem L
       (czyli niekoniecznie spójną podlistą)
       (preferowane rozwiązanie: każdy podciąg wygenerowany jeden raz)

    j) `wypisz(L)` == czytelne wypisanie elementów listy L, z zaznaczeniem
       jeśli lista pusta (np. elementy oddzielane przecinkami, po
       ostatnim elemencie kropka)

    k) sortowanie przez wstawianie:

~~~
         insertionSort(Lista, Posortowana),
         insert(Lista, Elem, NowaLista)
~~~
