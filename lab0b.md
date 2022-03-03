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

5. Ćwiczymy funkcje wyzszego rzedu

    a. Napisz funkcje

    ```
	incAll :: [[Int]] -> [[Int]]
    ```

    która zwiększy o 1 każdy element każdego elementu swojego argumentu, np

	*Main Data.List> incAll $ inits [1..3]
	[[],[2],[2,3],[2,3,4]]

    b. Napisz przy pomocy `foldr` (nadaj własne nazwy)

       * silnię (w sumie było na wykładzie)
       * `and :: [Bool] -> Bool`
       * `concat :: [[a]] -> [a]`

    <!-- 
       * `maximum :: [Int] -> Int`
       * `minimum :: [Int] -> Int`
       * uogólnij dwie powyższe przy uzyciu `winner :: (a -> a -> a) -> [a] -> a`
    -->

    c. Napisz nub przy pomocy filter
