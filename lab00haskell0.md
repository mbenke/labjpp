## Haskell

Na własnym laptopie można zainstalować GHC(i) korzystając z narzędzia [`ghcup`](https://www.haskell.org/ghcup/) np.

```
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

<!--
Alternatywą jest `stack`: http://haskellstack.org np.

```
curl -sSL https://get.haskellstack.org/ | sh
stack setup
stack ghci
```

Wtedy kompilować można przez `stack ghc -- argumenty` albo

```
$ PATH=$PATH:$(stack path --compiler-bin)
```
-->

### ghci
 + uruchamianie
 + obliczanie wyrażeń
 + podstawowe komendy: :h[elp] :t[ype] :l[oad] :r[eload] :q[uit
 + odkrywamy Haskell (notatki na moodle)
ew.  tutorial: [Learn You A Haskell](http://learnyouahaskell.com/starting-out)

## Emacs
 + klawiszologia: C = Control, M = Meta (Escape lub Alt) S = Shift (E-M-A-C-S)
 + wyjście: `C-x C-c`
 + tutorial `C-h t`
 + otwarcie (kolejnego) pliku `C-x C-f`
 + przejście do innego bufora `C-x b` lub `C-x C-b`
 + podział okna: w pionie `C-x 2` w poziomie `C-x 3` cofniecie `C-x 1` lub `C-x 0`
 +  przejscie do kolejnego fragmentu okna `C-x o`
 + zaznaczanie `C-spacja`, kopiowanie `M-w` wycinanie `C-w` wklejanie `C-y`  
 + więcej: [EmacsWiki](http://www.emacswiki.org/emacs/), [UChicago Emacs tutorial](http://www2.lib.uchicago.edu/keith/tcl-course/emacs-tutorial.html)
 + dla tych, którzy wczesniej uzywali vima, jest [evil mode](http://www.emacswiki.org/emacs/Evil) i [spacemacs](https://github.com/syl20bnr/spacemacs)

### haskell-mode
 + [instalacja](https://github.com/haskell/haskell-mode/blob/master/README.md#packageel-based-installation)
 + Podczas edycji pliku  .hs `C-c C-l` ładuje go w interpreterze (ew. dzieląc okno), `C-x o` przełącza do drugiego okna. 

## VS Code
Można użyć rozszerzenia `Haskell`, ale trzeba uważać z autouzupełnianiem.

Warto zwrócić uwagę na obliczanie wyrażeń przy użyciu  `-- >>>`  w kodzie (prawie jak doctest)
## Odkrywamy Haskell
Przećwiczyć elementy zawarte w [notatkach](https://moodle.mimuw.edu.pl/mod/resource/view.php?id=129099): obliczanie wyrażeń,wycinanki listowe, definiowanie funkcji, silnia, let, lambda, operatory


### Proste zadania

1. Napisz funkcję `countdown`,  która dla danej liczby naruralnej będzie "odliczać" od tej liczby do 0, np.

```
> countdown 9
[9,8,7,6,5,4,3,2,1,0]
```

2. Napisz funkcję `collatz`, która dla danej liczby da jej [sekwencję Collatza](https://pl.wikipedia.org/wiki/Problem_Collatza), np 

```
> collatz 17
[17,52,26,13,40,20,10,5,16,8,4,2,1]
```

3. Standardowa funkcja `last` daje ostatni element swojego argumentu. Czy potrafisz napisać własną implementację tej funkcji?

### Zadania (raczej po wykładzie)

1. Napisz własne odpowiedniki standardowych funkcji `head, tail, ++, take, drop, filter, map, concat`

2. Napisz funkcję `inits`, ktora dla danej listy da listę wszystkich jej odcinków początkowych, np.

    ~~~~
    inits [1,2] == [[],[1],[1,2]]
    ~~~~

3. Napisz funkcje `partitions`, ktora dla danej listy `xs` da liste wszystkich par `(ys,zs)` takich, że  

    ~~~~
    xs == ys ++ zs
    ~~~~

4. Napisz funkcje `permutations`, która dla danej listy da listę wszystkich jej permutacji (dla uniknięcia niejasności możemy założyć, ze wszystkie elementy listy wejściowej są różne)

5. Napisz funkcje `nub`, ktora usunie z listy wszystkie duplikaty, np

    ~~~~
    nub [1,2,1,3,1,2,1,4] == [1,2,3,4]
    ~~~~

    Możliwe jest wiele rozwiazań, ale przyjmijmy, że funkcja nub pozostawia pierwsze wystąpienie danej wartości, a usuwa powtórzenia.

Po skończeniu działu 'Haskell 0' można ew. zacząć zadania z 'Haskell 1'
