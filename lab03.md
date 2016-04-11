## Zadanie 1
W poprzednim tygodniu pisaliśmy funkcje

    readInts2 :: String -> Either String [Int]
    sumInts :: String -> String

Przepisz funkcje readInts2, sumInts  na notację monadyczną

(Uwaga: w tym zadaniu raczej nadal tworzymy funkcje String -> String i korzystamy z interact niz bezposrednio z IO, chyba ze ktos bardzo chce)

## Zadanie 2 - IO

a. Napisz program który wypisze swoje argumenty, każdy w osobnej linii

b. Napisz program, który będzie pytał użytkownika o ulubiony język programowania, tak długo aż odpowiedzią będzie 'Haskell' ;)

c. Napisz uproszczoną wersję programu wc (wypisującą ilość linii, słów i znaków w pliku o nazwie podanej jako argument)


## Zadanie 3. 

Uzupełnić przykład z wykładu:

    data ParseError = Err {location::Int, reason::String}
    instance Error ParseError ...
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

## Zadanie 4. Operacje monadyczne

Napisz własną implementację funkcji

    sequence :: Monad m => [m a] -> m [a] 
    mapM :: Monad m => (a -> m b) -> [a] -> m [b]
    forM :: Monad m => [a] -> (a -> m b) -> m [b]

## Zadanie 5. (opcjonalne)

Nieco inny od monad model obliczeń reprezentuje klasa Applicative:

    class Functor f => Applicative f where
      pure :: a -> f a
      (<*>) :: f(a->b) -> f a -> f b 

- `pure` to to samo co `return`
- Operator `(<*>)` reprezentuje sekwencjonowanie obliczeń podobne do `(=<<)` 
z tym, że kolejne obliczenie nie zależy od wyniku poprzedniego (choć jego wynik oczywiście może).
- Dla każdej monady można zdefiniować instancję Applicative:

        pure = return
        mf <*> ma =  do { f <- mf; a <- ma; return mf ma }


a. Zdefiniuj instancje `Applicative` dla `Maybe` i `Either`
b. Zdefiniuj operację `*>` będącą analogiem `>>` (czyli ignorującą wynik pierwszego obliczenia):

    (*>) :: f a -> f b -> f b

c. Zdefiniuj analogiczną operację ignorującą wynik drugiego obliczenia:

    (<*) :: f a -> f b -> f a


d. Spróbuj wykonać zadanie 3 używając `Applicative` zamiast `Monad`
