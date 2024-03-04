1. Napisz funkcję `triples  :: Int -> [(Int,Int,Int)]`, która dla argumentu n da listę wszystkich trójek liczb naturalnych o elementach z `[1..n]`

2. Napisz funkcję `triads :: Int -> [(Int,Int,Int)]`, ktora da liste trojek pitagorejskich  ($x^2 + y^2 = z^2$ dla x,y,z <= n)

    W pierwszym podejsciu dostaniemy np.

    ~~~~
    *Main> triads 20
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

3. Można zrobic 
 + liczenie liczb pierwszych na rozne sposoby
 + Liczby Fibonacciego
 + Rekursja ogonowa -- silnia z akumulatorem, liczby Fibbonacciego, reverse
   
    Funkcję obliczającą sumę listy
    ``` haskell
    sum1 [] = 0
    sum1 (x:xs) = x + sum xs
    ```
    możemy uogólnić do
    ``` haskell
    -- sum2 m xs = m + sum xs
    sum2 m [] = m
    sum2 m (x:xs) = sum2 (m+x) xs

    sum3 = sum2 0
    ```
    Rekurencja w funkcji `sum2` jest "ogonowa", co odpowiada iteracji.
    Napisz w podobny sposób
    - silnię  (specyfikacja: `fac' n m= n!*m`)
    - funkcję odwracającą listę (`rev' xs ys = rev xs ++ ys`)
    - liczby Fibonacciego (trochę trudniejsze)

4. Napisz "bezpieczne" warianty funkcji `head, tail, last`, dające `Nothing` dla listy pustej.
    ``` haskell
    safeHead :: [a] -> Maybe a
    safeTail :: [a] -> Maybe [a]
    safeLast :: [a] -> Maybe a
    ```

6. Napisz funkcję

    ~~~~
    indexOf :: Char -> String -> Maybe Int
    ~~~~

    taką, że jeśli c wystepuje w s na pozycji n, to `(indexOf c s)  = Just n`, wpp `Nothing`, np

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

    taką, że `(positions c s)` daje listę wszystkich pozycji, na których c wystepuje w s, np.

    ~~~~
    *Main> positions 'a' "Ala ma kota"
    [2,5,10]
    *Main> positions 'b' "Ala ma kota"
    []
    ~~~~

7. Ćwiczymy funkcje wyzszego rzedu

    a. Napisz funkcje
    ```
    incAll :: [[Int]] -> [[Int]]
    ```
    która zwiększy o 1 każdy element każdego elementu swojego argumentu, np
    ```
    > incAll $ inits [1..3]
    [[],[2],[2,3],[2,3,4]]
    ```
    b. Napisz przy pomocy foldl/foldr

    *   silnię
    *   `concat :: [[a]] -> [a]`
    * `maximum :: [Int] -> Int`
    * `minimum :: [Int] -> Int`
    * uogólnij dwie powyższe przy uzyciu `winner :: (a -> a -> a) -> [a] -> a`

    c. Napisz `nub` (funkcję eliminującą duplikaty z listy) przy pomocy `filter`

    d. Napisz funkcję obliczającą iloczyn skalarny dwóch list liczb; użyj `zipWith`


8. Zapoznać się z [Data.Map](http://hackage.haskell.org/package/containers-0.5.6.3/docs/Data-Map-Lazy.html)


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
