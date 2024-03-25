Uwaga: na poniższe zadania mozna przeznaczyć 3, a w miarę potrzeby i
mozliwości nawet 4 zajęcia.

## Zadanie 1. Reader

a. Dany typ drzew

``` haskell
 data Tree a = Empty | Node a (Tree a) (Tree a) deriving (Eq, Ord, Show)
```

Napisz funkcję

``` haskell
  renumber :: Tree a -> Tree Int
```
która dla danego drzewa da drzewo, gdzie w każdym węźle przechowywana będzie głębokość tego węzła (odległość od korzenia).

Porównaj rozwiązania z użyciem monady Reader i bez.

b. Dane typy dla wyrażeń arytmetycznych

``` haskell
    type Var = String
    data Exp = EInt Int
         | EOp  Op Exp Exp   --  albo EAdd Exp Exp  | ESub Exp Exp | ...
         | EVar Var
         | ELet Var Exp Exp  -- let var = e1 in e2

    data Op = OpAdd | OpMul | OpSub
```

Napisz funkcję 

``` haskell
    evalExp :: Exp -> Int
```

ktora obliczy wartość takiego wyrażenia, np

``` haskell
--
--      let x =
--          let y = 6 + 9
--          in y - 1
--      in x * 3
-- 
-- ==>  42
--
test = ELet "x" (ELet "y" (EOp OpAdd (EInt 6) (EInt 9))
                      (EOp OpSub y (EInt 1)))
                (EOp OpMul x (EInt 3))
    where x = EVar "x"
          y = EVar "y"

```

(można też użyć typu wyrażeń z jednego z poprzednich labów)

Użyj monady czytelnika środowiska (Reader). Środowisko może być
np. jednego z typów

```
Map Var Int
[(Var, Int)]
Var -> Maybe Int
```

## Zadanie 2. Monada List

Funkcja

    allPairs :: [a] -> [a] -> [[a]]
    allPairs xs ys = [[x,y] | x <- xs, y <- ys]

daje listę wszystkich list dwuelementowych, gdzie pierwszy element
należy do pierwszego argumentu, drugi do drugiego, np.

~~~~
*Main> allPairs [1,2,3] [4,5]
[[1,4],[1,5],[2,4],[2,5],[3,4],[3,5]]
~~~~

 - przepisz na notację monadyczną
 - uogólnij te funkcję do `allCombinations :: [[a]] -> [[a]]`, która dla n-elementowej listy list da listę wszystkich n-elementowych list takich, że i-ty element należy do i-tego elementu argumentu, np

~~~~
Main> allCombinations [[1,2], [4,5], [6], [7]]  
[[1,4,6,7],[1,5,6,7],[2,4,6,7],[2,5,6,7]]
~~~~

Jak najprościej zapisać `allCombinations` przy pomocy poznanych operacji monadycznych?

## Zadanie 3. Monada State

a. Dany typ drzew

``` haskell
data Tree a = Empty | Node a (Tree a) (Tree a) deriving (Eq, Ord, Show)
```

Napisz funkcję

``` haskell 
renumberTree :: Tree a -> Tree Int
```
która ponumeruje wezly drzewa tak, ze kazdy z nich bedzie mial inny numer.
Porownaj rozwiazania z uzyciem monady State i bez.

możliwe dodatkowe wymaganie: ponumeruj wezly drzewa w kolejnosci infiksowej.

~~~~
(toList $ renumber $ fromList "Learn Haskell") == [0..12]
~~~~

b. Korzystając z monady State, napisz interpreter języka Tiny
(patrz przedmiot Semantyka i Weryfikacja Programów)

~~~
Exp:    E ::= x | n | E + E | ...
BExp:   B ::= E == E | ...
Stmt:   S ::= skip | x := E | S1;S2
        | if B then S1 else S2 | while B do S
~~~

Można użyć np. typu

``` haskell
type IM a = State IntState a
```

gdzie `IntState` jest typem stanu interpretera.

Napisz funkcję

``` haskell
execStmt :: Stmt -> IO ()
```

która wykona podaną instrukcję (program) i wypisze stan końcowy (w tym
wypadku wartości zmiennych)

c. Następny krok to dodanie deklaracji zmiennych lokalnych:

~~~
Stmt: S ::= ... | begin [D] S end
Decl: D ::= var x=e;
~~~

Tutaj dla obliczania `Stmt` najlepiej użyć jednocześnie monady Reader i State:
część Reader odczytuje funkcje z `Var` w nowy typ "lokacji pamięci" `Loc`
(wystarczy `type Loc=Int`), a część `State` operuje na mapowaniach
z `Loc` w `Int`. Trzeba też zaimplementować funkcję
`alloc :: (Map Loc Int) -> Loc`, która zwraca nieużywaną lokację.

