## Zadanie 1
W poprzednim tygodniu pisaliśmy funkcje

``` haskell
    readInts2 :: String -> Either String [Int]
    sumInts :: String -> String
```

```
*Main> readInts2 "1 23 456 abc 9"
Left "Not a number: abc"
*Main> sumInts "1 23 456 abc 9"
"Not a number: abc"
*Main> sumInts "1 2 3"
"6"
```
Zamiast `case ... of {Left -> ...; Right -> ...}` użyj w nich operacji monadycznych lub `do`. Można  też użyć `readMaybe` lub `readEither`.

```
Prelude> import Text.Read
Prelude Text.Read> :t readMaybe
readMaybe :: Read a => String -> Maybe a
Prelude Text.Read> :t readEither
readEither :: Read a => String -> Either String a
```


## Zadanie 2

a. Zaimplementuj i uruchom przykład z wykładu

``` haskell
data Exp = Val Int | Div Exp Exp
safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv x y = Just (div x y)

eval :: Exp -> Maybe Int
eval (Val n)   = return n
eval (Div x y) = do 
    n <- eval x
    m <- eval y
    safediv n m 
```

uzupełnij ten przykład o inne operacje arytmetyczne.

b. Napisz funkcje obliczające wartość listy wyrażeń:

```
evalList' :: [Exp] -> [Maybe Int]
evalList :: [Exp] -> Maybe [Int]
```

c. Zmodyfikuj kod z (a) i (b) używając `Either` zamiast `Maybe`:

``` haskell
safediv :: Int -> Int -> Either String Int
safediv _ 0 = Left "Division by zero"
safediv x y = Right (div x y)

eval :: Exp -> Either String Int
```
d. Rozszerz wyrażenia o

``` haskell
data Exp = ... Var String
```

Napisz funkcję 

``` haskell 
eval :: Env -> Exp -> Either String Int
```

W przypadku użycia w wyrazeniu nieznanej zmiennej zgłoś odpowiedni błąd.

Można przyjąć `Env = [(String, Int)]` lub `Data.Map.Map String Int`.


## Zadanie 3 

Uzupełnić przykład z wykładu:
``` haskell
import Control.Monad.Error.Class
data ParseError = Err {location::Int, reason::String}

type ParseMonad = Either ParseError
parseHexDigit :: Char -> Int -> ParseMonad Integer
parseHex :: String -> ParseMonad Integer
toString :: Integer -> ParseMonad String

-- convert zamienia napis z liczba szesnastkowa 
--   na napis z liczba dziesietna
convert :: String -> String
convert s = str where
  (Right str) = tryParse s `catchError` printError
  tryParse s = do {n <- parseHex s; toString n}
  printError e = ...
```

Wskazówka: to zadanie, chociaż wygląda groźnie nie jest bardzo odległe od `sumInts` w poprzednim zadaniu.

``` haskell
import Data.Char(isHexDigit, digitToInt)
isHexDigit :: Char -> Bool
digitToInt :: Char -> Int
```

```
Prelude Data.Char> digitToInt '7'
7
Prelude Data.Char> digitToInt 'f'
15
Prelude Data.Char> digitToInt 'F'
15
Prelude Data.Char> digitToInt 'z'
*** Exception: Char.digitToInt: not a digit 'z'
```

## Zadanie 4 - IO

a. Napisz program który wypisze swoje argumenty, każdy w osobnej linii (wskazówka: `System.Environment.getArgs`)

b. Napisz program, który będzie pytał użytkownika o ulubiony język programowania, tak długo aż odpowiedzią będzie 'Haskell' ;)

c. Napisz uproszczoną wersję programu `wc` (wypisującą ilość linii, słów i znaków w pliku o nazwie podanej jako argument, bądź `stdin` jeśli bez argumentu).

Wskazówki:

``` haskell
getLine :: IO String
putStrLn :: String -> IO ()

import System.Environment
getArgs :: IO [String]
getProgName :: IO String

import System.IO
withFile :: FilePath -> IOMode -> (Handle -> IO r) -> IO r
ReadMode :: IOMode
hGetContents :: Handle -> IO String
```
