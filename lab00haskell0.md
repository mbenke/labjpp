### Emacs
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
 
### Haskell
Instalacja na własnym laptopie: zalecane zainstalowanie stack - https://haskellstack.org

Po instalacji
```
$ stack setup # to potrwa chwilę
$ stack ghci
```

Kompilować można przez `stack ghc -- argumenty` albo

```
$ PATH=$PATH:$(stack path --compiler-bin)
```


### ghci

 + uruchamianie
 + obliczanie wyrażeń
 + podstawowe komendy: :h[elp] :t[ype] :l[oad] :r[eload] :q[uit]
 + odkrywamy Haskell (notatki na moodle)
ew.  tutorial: [Learn You A Haskell](http://learnyouahaskell.com/starting-out)

### haskell-mode
 + [instalacja](https://github.com/haskell/haskell-mode/blob/master/README.md#packageel-based-installation)
 + Podczas edycji pliku  .hs `C-c C-l` ładuje go w interpreterze (ew. dzieląc okno), `C-x o` przełącza do drugiego okna. 

### Odkrywamy Haskell
Przećwiczyć elementy zawarte w [notatkach](https://moodle.mimuw.edu.pl/mod/resource/view.php?id=15438): obliczanie wyrażeń,wycinanki listowe, definiowanie funkcji, silnia, let, lambda, operatory

### Zadania (raczej po wykładzie)

1. Napisz własne odpowiedniki standardowych funkcji head, tail, ++, take, drop

2. Napisz funkcję `inits`, ktora dla danej listy da liste wszystkich jej odcinkow poczatkowych, np.

    ~~~~
    inits [1,2] == [[],[1],[1,2]]
    ~~~~

3. Napisz funkcje `partitions`, ktora dla danej listy `xs` da liste wszystkich par `(ys,zs)` takich, że  

    ~~~~
    xs == ys ++ zs
    ~~~~

4. Napisz funkcje permutations, ktora dla danej listy da listę wszystkich jej permutacji (dla unikniecia niejasności mozemy założyć, ze wszystkie elementy listy wejściowej sa różne)

5. Napisz funkcje nub, ktora usunie z listy wszystkie duplikaty, np

    ~~~~
    nub [1,2,1,3,1,2,1,4] == [1,2,3,4]
    ~~~~

    Możliwe jest wiele rozwiazan, ale przyjmijmy, że funkcja nub pozostawia pierwsze wystąpienie danej wartości, a usuwa powtorzenia.

Po skończeniu działu 'Haskell 0' można ew. zacząć zadania z 'Haskell 1'
