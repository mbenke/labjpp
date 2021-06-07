# Smalltalk 3b - tasowanie i rozdawanie

## Zadanie

Zdefiniuj klasę `Rozdanie`, której obiekty będą reprezentowały (brydżowe) rozdanie talii 52 kart czterem graczom. Zaimplementuj metodę klasową `#new` tworzącą losowe rozdanie oraz obiektowe metody `#printOn:` i `#displayOn:` wypisujące dwa różne tekstowe zapisy rozdania.

Wykonanie fragmentu kodu:

    r := Rozdanie new.
    s1 := r printString.
    s2 := r displayString

powinno umieścić na zmiennej "s1" reprezentację rozdania w formacie:

    'N(6 trefl, 8 trefl, 2 karo, 5 karo, 9 karo, 5 kier, 6 kier, 9 kier, Krol kier, 3 pik, 4 pik, 9 pik, Walet pik)
    E(10 trefl, Krol trefl, As trefl, 4 karo, 7 karo, 10 karo, As karo, 3 kier, 7 kier, Walet kier, Dama kier, 10 pik, Dama pik)
    S(2 trefl, 4 trefl, 5 trefl, 7 trefl, Walet trefl, Dama trefl, 3 karo, Krol karo, 2 kier, 4 kier, 5 pik, 7 pik, As pik)
    W(3 trefl, 9 trefl, 6 karo, 8 karo, Walet karo, Dama karo, 8 kier, 10 kier, As kier, 2 pik, 6 pik, 8 pik, Krol pik)'

a na zmiennej "s2":

    '(N)
    P    W 9 4 3
    C    K 9 6 5
    K    9 5 2
    T    8 6

    (E)
    P    D 10
    C    D W 7 3
    K    A 10 7 4
    T    A K 10

    (S)
    P    A 7 5
    C    4 2
    K    K 3
    T    D W 7 5 4 2

    (W)
    P    K 8 6 2
    C    A 10 8
    K    D W 8 6
    T    9 3
    '

## Słowniki

Będziemy musieli ułożyć karty kolorami. Przyda nam się do tego słownik (można ew. uzyć `LookupTable`)

```
Dictionary new at: #trefl put: 1; at: #kier put: 3; yourself
a Dictionary(#trefl -> 1 #kier -> 3)

'kier' asSymbol
#kier
```

ale ponieważ kolory mają wartości liczbowe, to mozna uzyć też po prostu tablicy, zwlaszcza gdy nie mamy podklas dla poszczególnych kolorów.

## Tasowanie

Poprzednio udało nam się stworzyc kolekcję obejmującą wszystkie kart w talii:

```
Karta wszystkie asArray collect: [ :w | w printString] 
#('2 trefl' '3 trefl' '4 trefl' '5 trefl' '6 trefl' '7 trefl' '8 trefl' '9 trefl' '10 trefl' 'Walet trefl' 'Dama trefl' 'Krol trefl' 'As trefl' '2 karo' '3 karo' '4 karo' '5 karo' '6 karo' '7 karo' '8 karo' '9 karo' '10 karo' 'Walet karo' 'Dama karo' 'Krol karo' 'As karo' '2 kier' '3 kier' '4 kier' '5 kier' '6 kier' '7 kier' '8 kier' '9 kier' '10 kier' 'Walet kier' 'Dama kier' 'Krol kier' 'As kier' '2 pik' '3 pik' '4 pik' '5 pik' '6 pik' '7 pik' '8 pik' '9 pik' '10 pik' 'Walet pik' 'Dama pik' 'Krol pik' 'As pik')
```

Do tasowania/rozdawania kart będzie nam potrzebne losowanie. Używalismy go już poprzednio:

```
Random new next
0.6133594264338535
```

...ale nam potrzebne będzie losowanie liczb całkowitych. W klasie `Random` nie ma metody `nextInteger:`. Możemy stworzyć własną podklasę, albo dodac swoją metodę do klasy `Random`:

``` smalltalk
nextInteger: m
	^(self next * m) truncated + 1
```

```
Random new nextInteger: 52
21

rnd := Random new. (1 to: 10) collect:[:x | rnd nextInteger: 6]
#(4 2 3 6 5 1 2 6 2 2)
```

Teraz możemy posortować talię uzywając  [algorytmu Fisher-Yates w prezentacji Knutha](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle#The_modern_algorithm)

``` smalltalk
rnd := Random new. 
talia := Karta wszystkie asArray. 
talia size to: 2 by: -1 do: [ :i | | tmp | 
    j := rnd nextInteger: i.
    tmp:= talia at: i.
    talia at: i put: (talia at: j).
    talia at: j put: tmp
]. 
talia collect: [ :k | k printString ]
#('Krol trefl' '9 pik' 'Walet trefl' '6 pik' '7 karo' 'Walet karo' '3 pik' '8 kier' '2 pik' 'Krol karo' '10 kier' '2 trefl' 'Krol pik' 'As trefl' '9 kier' '5 kier' 'As pik' '5 karo' 'Walet kier' '4 pik' '8 pik' 'Dama trefl' '6 kier' '5 trefl' 'As kier' '2 kier' '3 trefl' '2 karo' '4 trefl' '10 trefl' 'Krol kier' '6 karo' 'Dama kier' 'Dama karo' '6 trefl' 'Dama pik' '9 trefl' '10 pik' '10 karo' '8 karo' '9 karo' '5 pik' 'As karo' '8 trefl' '4 kier' '7 pik' '7 kier' '3 karo' '4 karo' '7 trefl' 'Walet pik' '3 kier')
```

Umieśćmy teraz to wszystko w klasie `Talia` z metodami `#tasuj`, `#collect:` i innymi potrzebnymi (możnaby dziedziczyć po Collection, ale wtedy trzeba zaimplementować metody #add:, #do: i #remove:ifAbsent:).

```
Talia new collect: [ :k | k printString ]
an OrderedCollection('2 trefl' '3 trefl' '4 trefl' '5 trefl' '6 trefl' '7 trefl' '8 trefl' '9 trefl' '10 trefl' 'Walet trefl' 'Dama trefl' 'Krol trefl' 'As trefl' '2 karo' '3 karo' '4 karo' '5 karo' '6 karo' '7 karo' '8 karo' '9 karo' '10 karo' 'Walet karo' 'Dama karo' 'Krol karo' 'As karo' '2 kier' '3 kier' '4 kier' '5 kier' '6 kier' '7 kier' '8 kier' '9 kier' '10 kier' 'Walet kier' 'Dama kier' 'Krol kier' 'As kier' '2 pik' '3 pik' '4 pik' '5 pik' '6 pik' '7 pik' '8 pik' '9 pik' '10 pik' 'Walet pik' 'Dama pik' 'Krol pik' 'As pik')

Talia new tasuj collect: [ :k | k printString ]
an OrderedCollection('As kier' '8 trefl' '3 trefl' '9 karo' 'As pik' '9 kier' 'Krol pik' '7 karo' '5 trefl' 'Walet karo' '5 pik' '10 kier' '7 kier' '3 kier' '5 kier' '6 karo' '8 kier' '6 trefl' 'Dama karo' '10 trefl' '2 trefl' '7 trefl' '2 pik' '6 kier' '10 karo' '4 karo' '2 karo' 'As karo' '4 kier' '8 pik' '5 karo' 'Walet kier' '4 pik' '6 pik' 'Dama trefl' 'Walet pik' '2 kier' 'Krol kier' '4 trefl' 'Krol trefl' '7 pik' '10 pik' 'Krol karo' 'As trefl' 'Dama kier' '9 pik' '3 pik' '8 karo' 'Walet trefl' 'Dama pik' '3 karo' '9 trefl')
```

Użycie `OrderedCollection` zamiast `Array` pozwoli nam na rozdawanie przy użyciu `removeFirst`:

```
talia:= Talia new tasuj. 
r := OrderedCollection new. 13 timesRepeat:  [ r add: talia removeFirst ].
r collect: [ :k | k printString ]
an OrderedCollection('2 trefl' '10 trefl' 'Dama karo' '8 trefl' 'Dama pik' '7 karo' 'Dama trefl' '7 trefl' '4 trefl' 'As pik' 'Walet kier' '5 karo' '5 kier')
```

(można dodać też metodę `#wybierz: n`):

```
(Talia new tasuj wybierz: 13) collect: [ :k | k printString ]
```

Ręka jest określona przez jej nazwę i karty. Warto użyć `SortedCollection` do przechowywania kart, zeby były odpowiednio ułożone w kolejności

```
(Reka nazwa: $N karty: (Talia new tasuj wybierz: 13) ) karty
a SortedCollection(a Blotka trefl a Figura trefl a Figura trefl a Blotka karo a Blotka karo a Blotka karo a Figura karo a Blotka kier a Blotka kier a Blotka kier a Figura kier a Blotka pik a Figura pik)
```

(nie mamy jeszcze metody `#displayString` a `collect:` zmieni kolekcję na `OrderedCollection`)

```
(Reka nazwa: $N karty: (Talia new tasuj wybierz: 13) ) karty collect: [:k | k printString]an OrderedCollection('2 trefl' 'Walet trefl' '5 karo' 'Walet karo' '4 kier' '10 kier' 'Walet kier' '3 pik' '6 pik' '7 pik' '10 pik' 'Walet pik' 'As pik')
```

Przed użyciem `SortedCollection` warto upewnic sie, że, karty, kolory i wartości mają zdefiniowane metody porównania.

Teraz możemy już zdefiniować `Rozdanie` zawierające ręce N,E,S,W.

