## Podstawowe informacje o operacjach wejścia/wyjścia w Prologu

1. Termowe operacje wejścia/wyjścia, standardowe predykaty:
      * `write(Term)` - wypisanie termu (na bieżący plik wyjściowy)
      * `nl`  - wypisanie znaku końca wiersza (na bieżący plik wyjściowy)
            (równoważne wywołaniu: `write('\n')`)
      * format('<opis formatu>', ListaElementów)
          wypisanie wszystkich elementów, zgodnie z podanym opisem
          znaki sterujące: poprzedzone `~` (np. `~d`, `~n`)
          np. `format('Proces ~d: ~d~n', [Id, Licznik])`
2. Wczytywanie
      * `read(Term)` - wczytanie (z bieżącego pliku wejściowego, domyślnie stdin) dowolnego
          (poprawnego składniowo) termu, zakończonego kropką;
          np. `read(program(ListaInstrukcji))`
      * zmiana znaku zachęty ze standardowego `|:` przy użyciu `prompt/2`; np. `prompt(_,' ')`
      * `compile(NazwaPliku)` - wczytanie klauzul z pliku, np `compile(`graph.txt`).
 
3. (Elementarne) operacje na plikach
     - otwarcie pliku (do czytania):    see(NazwaPliku)
     - zamknięcie pliku (do czytania):  seen
