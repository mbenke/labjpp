## Wstęp

Kompilacja programu przy użyciu ghc; na razie nie mówimy jeszcze o IO, wystarczy 

 - z użyciem interact

~~~~
	smain :: String -> String
	main = interact smain
~~~~
- lub, dla prostszych programów, z `putStrLn` bądź `print`:

~~~~
	mymain :: String 
	main = putStrLn mymain
~~~~

~~~~
    main = interact smain

    smain :: String -> String
    smain input = "Hello world\n"

    $ runhaskell hello.hs 
    Hello world
    ben@trawa:$ ghc -o hello --make hello.hs
    [1 of 1] Compiling Main             ( hello.hs, hello.o )
    Linking hello ...

    $ ./hello
    Hello world
~~~~

Przy okazji można napisać funkcje

~~~~
showInt :: Int -> String
showIntLst :: [Int] -> String
~~~~
i ogólniejszą:

~~~~
    showLst :: (a -> String) -> [a] -> String
~~~~

(Uwaga: ta funkcja nie może się nazywać showList, bo jest juz tak funkcja w Prelude)
Wskazówki: `succ '0' == '1'` itd; dzielenie całkowite to `div`

~~~~
    import Data.Char(ord,chr)   
~~~~

Napisz program, który podzieli tekst otrzymany na wejściu na linie długości nie większej niż zadana stała (np. 30)


## Zadanie 1

Napisz funkcje

```
foldEither :: (a  -> c) -> (b -> c) -> Either a b -> c
mapEither :: (a1 -> a2) -> (b1 -> b2) -> Either a1 b1 -> Either a2 b2
mapRight ::  (b1 -> b2) -> Either a b1 -> Either a b2
```

oraz

```
    reverseRight :: Either e [a] -> Either e [a]
```

odwracajaca liste zawarta w `Right`

## Zadanie 2
Importy i moduły biblioteczne, np. przećwiczyć http://learnyouahaskell.com/modules

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
która da listę liczb, jeśli wszystkie słowa jej argumentu są liczbami
a komunikat o błędzie w przeciwnym przypadku

Może się przydać funkcja `reverseRight` (albo `mapRight`)

```
    *Main> readInts2  "1 23 456 foo 9"
    Left "Not a number: foo"
    *Main> readInts2  "1 23 456"     
    Right [1,23,456]
```

c. Napisz funkcję

    sumInts :: String -> String

- jesli  wszystkie slowa jej argumentu są liczbami da reprezentacje ich sumy
- wpp komunikat o bledzie

stwórz program uruchamiający funkcję `sumInts` przy pomocy `interact.`

## Zadanie 3
 
Rozważmy typ drzew troche inny niz na wykladzie

    data Tree a = Empty | Node a (Tree a) (Tree a) deriving (Eq, Ord, Show)

a. stwórz własne instancje `Eq`, `Show` 
 
~~~
instance Show a => Show (Tree a) where
   show t = ...

instance Eq a => Eq (Tree a) where
   t1 == t2 = ...
~~~

b. Napisz funkcje

    toList :: Tree a -> [a]

ktora zamieni drzewo w liste elementow drzewa (w porzadku infiksowym)

c. zaimplementuj drzewa BST z funkcjami

    insert :: (Ord a) => a -> Tree a -> Tree a
    contains :: (Ord a) -> a -> Tree a -> Bool
    fromList :: (Ord a) => [a] -> Tree a

d. Stwórz moduł drzew BST i wykorzystaj go w innym module do sortowania [Int]

- przy pomocy ghci
- przy pomocy ghc --make

## Zadanie 4
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

    testExp2 :: Exp
    testExp2 = (2 + 2) * 3

(metody abs i signum mogą mieć wartość undefined)

c. Napisz funkcję `simpl`, przekształcającą wyrażenie na równoważne prostsze wyrażenie
np. 

0*x + 1*y -> y

(nie ma tu precyzyjnej specyfikacji, należy uzyć zdrowego rozsądku; uwaga na zapętlenie).

d. Można dopisać liczenie pochodnej względem danej zmiennej:

~~~~
deriv :: String -> Exp -> Exp
~~~~

## Zadanie 4

a.Uzupełnij instancje klasy Functor dla Either e:

    import Prelude hiding(Either(..))
    data Either a b = Left a | Right b

    instance Functor (Either e) where
      -- fmap :: (a -> b) -> Either e a -> Either e b

b. skonstruuj instancje klasy `Functor` dla `Tree`

    -- class  Functor f  where
    --    fmap :: (a -> b) -> f a -> f b

    instance Functor Tree where...

    *Tree> fmap (+1) $ Node 1 Empty Empty
    Node 2 Empty Empty


c. napisz funkcję

    reverseRight :: Either e [a] -> Either e [a]

z użyciem `fmap`

d. Zdefiniuj klasę Pointed (funkcyjnych pojemników z singletonem)

    class Functor f => Pointed f where
      ppure :: a -> f a
 
i jej instancje dla list, Maybe, Tree



## Zadanie 5 (opcjonalne, dla znudzonych)


FIXME: to zadanie nie ma sensu w nowych werjsach GHC

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
