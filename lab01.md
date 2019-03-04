6. Napisz funkcję `triads :: Int -> [(Int,Int,Int)]`, ktora da liste trojek pitagorejskich  ($x^2 + y^2 = z^2$ dla x,y,z <= n)

    W pierwszym podejsciu dostaniemy np.

    ~~~~
    *Main> filter pyth $ triples 20
    [(3,4,5),(4,3,5),(5,12,13),(6,8,10),(8,6,10),(8,15,17),
      (9,12,15),(12,5,13),(12,9,15),(12,16,20),(15,8,17),(16,12,20)]
    ~~~~

    Teraz można dodać wymaganie, aby byly one nietrywialne, tzn
     - skoro (3,4,5) juz jest na liscie to (4,3,5) jest "trywialne"
     - skoro (3,4,5) juz jest na liscie to (6,8,10) i (12,16,20) sa "trywialne"

    ~~~~
    *Main> triads 20
    [(3,4,5),(5,12,13),(8,15,17)]
    ~~~~

7. Można zrobic
 + liczenie liczb pierwszych na rozne sposoby
 +Liczby Fibonacciego
 + Rekursja ogonowa -- silnia z akumulatorem, liczby Fibbonacciego, reverse

8. Napisz funkcję

    ~~~~
    indexOf :: Char -> String -> Maybe Int
    ~~~~

    taką, że jeśli c wystepuje w s na pozycji n, to (indexOf c s)  == Just n, wpp Nothing, np

    ~~~~
    *Main> indexOf 'a' "Ala"
    Just 2
    *Main> indexOf 'b' "Ala"
    Nothing
    ~~~~

    Napisz funkcje

    ~~~~
    positions :: Char -> String -> [Int]
    ~~~~

    taką, że (positions c s) daje listę wszystkich pozycji, na których c wystepuje w s, np.

    ~~~~
    *Main> positions 'a' "Ala ma kota"
    [2,5,10]
    *Main> positions 'b' "Ala ma kota"
    []
    ~~~~

9. Kompilacja programu przy użyciu ghc; na razie nie mówimy jeszcze o IO, wystarczy
     - z użyciem interact

    ~~~~
	smain :: String -> String
	main = interact smain
    ~~~~

     - lub, dla prostszych programów, z print:

    ~~~~
	mymain :: String
	main = putStrLn mymain
    ~~~~

    ~~~~
    main = interact smain

    smain :: String -> String
    smain input = "Hello world\n"
    ben@trawa:$ runhaskell hello.hs
    Hello world
    ben@trawa:$ ghc -o hello --make hello.hs
    [1 of 1] Compiling Main             ( hello.hs, hello.o )
    Linking hello ...
    ben@trawa: ./hello
    Hello world
    ben@trawa:~/Zajecia/Jpp/Slides/Pl/Haskell$
    ~~~~

    Przy okazji można napisać funkcje

    ~~~~
    showInt :: Int -> String
    ~~~~

    (wskazówki: succ '0' == '1' itd; dzielenie calkowite to div)

    ~~~~
    import Data.Char
    ord
    chr

    showIntLst :: [Int] -> String
    ~~~~

    i ogólniejszą:

    ~~~~
    showLst :: (a -> String) -> [a] -> String
    ~~~~

    (Uwaga: ta funkcja nie może się nazywać showList, bo jest juz tak funkcja w Prelude)

10. Napisz program, który podzieli tekst otrzymany na wejściu na linie długości nie większej niż zadana stała (np. 30)

1. Ćwiczymy funkcje wyzszego rzedu

a. Napisz funkcje

    incAll :: [[Int]] -> [[Int]]

która zwiększy o 1 każdy element każdego elementu swojego argumentu, np

    *Main Data.List> incAll $ inits [1..3]
    [[],[2],[2,3],[2,3,4]]

b, Napisz przy pomocy foldl/foldr

*    silnie
*    `concat :: [[a]] -> [a]`

c. Napisz nub przy pomocy filter


9. Zapoznać się z [Data.Map](http://hackage.haskell.org/package/containers-0.5.6.3/docs/Data-Map-Lazy.html)


~~~
 import qualified Data.Map as Map
 import Data.Map(map)

Prelude Map Data.Map> Map.empty
fromList []
Prelude Map Data.Map> insert "ola" 1 it

<interactive>:6:1:
    Not in scope: ‘insert’
    Perhaps you meant ‘Map.insert’ (imported from Data.Map)
Prelude Map Data.Map> Map.insert "ola" 1 it
fromList [("ola",1)]
Prelude Map Data.Map> Map.insert "ala" 2 it
fromList [("ala",2),("ola",1)]
Prelude Map Data.Map> Map.lookup "ola" it
Just 1
Prelude Map Data.Map> Map.lookup "ola" Map.empty
Nothing
~~~

i mutatis mutandis dla [Data.Set](http://hackage.haskell.org/package/containers-0.5.7.1/docs/Data-Set.html)
