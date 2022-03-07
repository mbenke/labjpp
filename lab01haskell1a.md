0. Zdefiniuj strumień (nieskończoną listę) wszystkich liczb naturalnych (było na wykładzie); wybierz kika jego elementów
1. Zdefiniuj strumienie liczb parzystych/nieparzystych
2. Wypróbuj strumień liczb Fibonacciego zdefiniowany na wykładzie
3. Zdefiniuj strumień wszystkich liczb pierwszych
4.  `Maybe` jest zdefiniowane jako 

    ```
    data Maybe a = Nothing | Just a
    ```
    Czyli jeśli `x::a` to `Just x :: Maybe a`

    Funkcje operujące na `Maybe` możemy definiować podobnie jak na listach (tyle, że bez rekurencji):
    ``` haskell
    size :: Maybe a -> Int
    size Nothing = 0
    size (Just _) = 1
    ```

    a. Napisz funkcję `incMaybe :: Maybe Int -> Maybe Int`

    ```
    > incMaybe (Just 41)
    Just 42

    > incMaybe Nothing
    Nothing
    ```
    
    b. Napisz funkcję `addMaybe :: Maybe Int -> Maybe Int -> Maybe Int`

    ```
    > addMaybe (Just 2) (Just 3)
    Just 5
    > addMaybe Nothing (Just 3)
    Nothing
    ```
    
 5. Bez używania standardowej funkcji `show` napisz funkcje

    ~~~~
    showInt :: Int -> String
    showIntLst :: [Int] -> String
    ~~~~

    (wskazówki: succ '0' == '1' itd; dzielenie calkowite to div)

    i ogólniejszą:

    ~~~~
    showLst :: (a -> String) -> [a] -> String
    ~~~~

    (Uwaga: ta funkcja nie może się nazywać showList, bo jest juz tak funkcja w Prelude)
