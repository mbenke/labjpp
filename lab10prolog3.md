Sposoby reprezentacji struktur danych
-----------------------------------

 (a) termowa (stałe, funkcje),

 (b) klauzulowa (klauzule unarne i nie tylko)


Drzewa binarne, BST
-------------------

Zdefiniować predykaty:

  a) `drzewo(D)` wtw, gdy D jest drzewem binarnym

  b) `insertBST(DrzewoBST, Elem, NoweDrzewoBST)`

  3 wersje tej procedury: jeśli element już jest w drzewie, to sukces (ponowne wstawienie elementu lub nie) lub porażka

  b') (ew. jeśli czas pozwoli albo zadanie do domu) deleteBST/3

  c) `wypiszBST(D)` = wypisanie na ekranie, porządek infiksowy

  d) `wypiszBST(D, L)` wtw, gdy L=lista wszystkich wierzchołków D
                                (porządek infiksowy)

  e) `stworzBST(L, D)` wtw, gdy D jest drzewem BST zawierającym wszystkie
                              elementy listy L (akumulator, ew. bez)

  f) `liscie(D, L)` wtw, gdy L = lista wszystkich liści, od lewej do prawej

  g) `wszerz(D, L)` wtw, gdy L = lista wszystkich wierzchołków wszerz

  h) `sortBST(L, S)` wtw, gdy S = lista posortowana, przy użyciu drzew BST


Grafy
-----

(skierowane, nieskierowane, cykliczne, acykliczne, etykietowane itd.)
(reprezentacja termowa oraz reprezentacja klauzulowa)


Grafy DAG (skierowane, acykliczne)
--------------------------------

Zdefiniować predykaty:

  a) `connect(A,B)`, `connect(Graf, A,B)` wtw, gdy istnieje ścieżka z A do B
       ścieżka = niepusty (!) ciąg krawędzi

  b) `path(A,B,P)` wtw, gdy P = opis ścieżki z A do B,
                      tzn. `P = [A, ..., B]`

Grafy skierowane, cykliczne
-------------------------

  c) `connect(A,B)` w dowolnym grafie (cyklicznym)

  d) euler - czy dany graf jest grafem Eulera, znalezienie (sprawdzenie)
     ścieżki Eulera (wprost z definicji): ścieżka, która przechodzi przez każdą jego krawędź dokładnie raz

Struktury otwarte i różnicowe
--------------------------

1. Implementacja kolejki FIFO, czyli:

     a) `empty(Kolejka)` - inicjacja kolejki (na pustą)

     b) `get(Elem, Kolejka, NowaKolejka)` - pobranie

     c) `put(Elem, Kolejka, NowaKolejka)` - wstawienie

     d) `empty(Kolejka)` - czy kolejka pusta

2. `wszerz(DrzewoBinarne, ListaWierzchWszerz)`
