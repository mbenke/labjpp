## Zadanie 1
Ćwiczymy funkcje wyzszego rzedu

a. Napisz funkcje

    incAll :: [[Int]] -> [[Int]]

która zwiększy o 1 każdy element każdego elementu swojego argumentu, np

    *Main Data.List> incAll $ inits [1..3]
    [[],[2],[2,3],[2,3,4]]

b, Napisz przy pomocy foldl/foldr

*    silnie
*    `concat :: [[a]] -> [a]`

c. Napisz nub przy pomocy filter

## Zadanie 2
 Rozważmy typ drzew troche inny niz na wykladzie

    Tree a = Empty | Node a (Tree a) (Tree a) deriving (Eq, Ord, Show)

a. skonstruuj instancje klasy Functor:

    -- class  Functor f  where
    --    fmap :: (a -> b) -> f a -> f b

    instance Functor Tree where...

    *Tree> fmap (+1) $ Node 1 Empty Empty
    Node 2 Empty Empty


b. Napisz funkcje

    toList :: Tree a -> [a]

ktora zamieni drzewo w liste elementow drzewa (w porzadku infiksowym)

c. zaimplementuj drzewa BST z funkcjami

    insert :: (Ord a) => a -> Tree a -> Tree a
    contains :: (Ord a) -> a -> Tree a -> Bool
    fromList :: (Ord a) => [a] -> Tree a

d. Stwórz modul drzew BST i wykorzystaj go w innym module do sortowania [Int]
- przy pomocy ghci
- przy pomocy ghc --make

## Zadanie 3
a.Uzupełnij instancje klasy Functor dla Either e:

    instance Functor (Either e) where
      -- fmap :: (a -> b) -> Either e a -> Either e b

b. napisz funkcje

    reverseRight :: Either e [a] -> Either e [a]

odwracajaca liste zawarta w Right
(dwie wersje, bezposrednio i z uzyciem fmap)

c. Zdefiniuj klasę Pointed (funkcyjnych pojemników z singletonem)

    class Functor f => Pointed f where
      pure :: a -> f a
 
i jej instancje dla list, Maybe, Tree

## Zadanie 4. 
Importy i moduły biblioteczne, np. przecwiczyć http://learnyouahaskell.com/modules

a. Napisz funkcję

    readInts :: String -> [Int]

która odczyta z napisu występujące w nim liczby naturalne, np

    *Main> readInts "1 23 456 7.8 abc 9"
    [1,23,456,9]
    *Main> readInts "foo"
    []

użyj funkcji `isDigit` z modulu `Data.Char` oraz funkcji `map`, `filter`, `all` z `Prelude`

b. Napisz podobną funkcję
    readInts2 :: String -> Either String [Int]
która da listę liczb, jeśli wszystkie slowa jej argumentu są liczbami
a komunikat o błędzie w przeciwnym przypadku

Może sie przydać funkcja `reverseRight` z zad. 3b (lub `fmap` dla `Either`)

    *Main> readInts2  "1 23 456 foo 9"
    Left "Not a number: foo"
    *Main> readInts2  "1 23 456"     
    Right [1,23,456]

c. Napisz funkcję

    sumInts :: String -> String

- jesli  wszystkie slowa jej argumentu są liczbami da reprezentacje ich sumy
- wpp komunikat o bledzie

stwórz program uruchamiający funkcję sumInts przy pomocy interact.

## Zadanie 5
Rozważmy typ dla wyrażeń arytmetycznych z let:

    data Exp 
      = EInt Int             -- stała całkowita       
      | EAdd Exp Exp         -- e1 + e2
      | ESub Exp Exp         -- e1 - e2
      | EMul Exp Exp         -- e1 * e2
      | EVar String          -- zmienna
      | ELet String Exp Exp  -- let var = e1 in e2

a. Napisz instancje Eq oraz Show dla Exp

b. Napisz instancje Num dla Exp tak, żeby można było napisać

ELE    testExp2 :: Exp
    testExp2 = (2 + 2) * 3

(metody abs i signum mogą mieć wartość undefined)

c. Napisz funkcję `simpl`, przekształcającą wyrażenie na równoważne prostsze wyrażenie
np. 

0*x + 1*y -> y

(nie ma tu precyzyjnej specyfikacji, należy uzyć zdrowego rozsądku; uwaga na zapętlenie).

## Zadanie 6 (opcjonalne, dla znudzonych)

Zdefiniuj klasę 

~~~~
infixl 4 <*>
class Pointed f => Applicative f where
  (<*>) :: f(a->b) -> f a -> f b 
~~~~

i jej instancje dla `Maybe`, [], tak aby spełnione były zależności

    pure id <*> v = v 

    pure (.) <*> u <*> v <*> w = u <*> (v <*> w) 

    pure f <*> pure x = pure (f x) 

    u <*> pure y = pure ($ y) <*> u 

Jaka będzie wartość wyrażenia

    [(+1),(*2)] <*> [1,2]
