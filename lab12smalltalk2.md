# Laboratorium: Smalltalk 2

## Zadanie 1

Zdefiniuj klasę `Stos` z obiektowymi metodami `#push:`, `#pop` i `#isEmpty`. Użyj listowej reprezentacji stosu.

## Zadanie 2

Zdefiniuj klasę `Kolejka` z obiektowymi metodami `#insert:`, `#remove` i `#isEmpty.` Użyj reprezentacji kolejki za pomocą dwóch stosów.

## Zadanie 3

Zdefiniuj klasę `TesterPierwszosci` z obiektową metodą `#jestPierwsza:` sprawdzającą, czy argument jest liczbą pierwszą.

Tester powinien spamiętywać w uporządkowanej kolekcji znane sobie liczby pierwsze, zaczynając od 2. 
Szukając odpowiedzi na pytanie, czy liczba jest pierwsza, będzie badał jej podzielność 
przez wszystkie liczby pierwsze nie przekraczające jej pierwiastka kwadratowego. 
Zacznie od tych, które pamięta, a jeśli kolekcja znanych mu liczb pierwszych okaże się nie wystarczająca, 
rozszerzy ją o dodatkowe liczby. Będzie je mógł wykorzystać, szukając odpowiedzi na ewentualne kolejne pytania.

Napisz metodę klasową `#default` w klasie `TesterPierwszosci`. 
Metoda ma dawać domyślny obiekt klasy `TesterPierwszosci` - za każdym razem ten sam.

Napisz metodę klasową `#jestPierwsza:` wykorzystujacą powyższa metodę `#default`.

W klasie `TesterPierwszosci` umieść metodę klasową `#test` sprawdzającą na pewnej liczbie losowych danych, 
czy `TesterPierwszosci` działa prawidłowo. 
Posłuż się klasą `Random` i klasową metodą `#primesUpTo:` z klasy `Integer`.

