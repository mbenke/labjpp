# Smalltalk 3a - Karty

## Cel
> Zdefiniuj klasę `Rozdanie`, której obiekty będą reprezentowały (brydżowe) rozdanie talii 52 kart czterem graczom. Zaimplementuj metodę klasową `#new` tworzącą losowe rozdanie oraz obiektowe metody `#printOn:` i `#displayOn:` wypisujące dwa różne tekstowe zapisy rozdania. [...]

Dzisiaj zdefiniujemy karty i jedną metodę wypisywania (`#printString`)
## Przygotowanie

Utwórz pakiet (np. `ZKarty`), w którym umieszczane będą wszystkie klasy zwiazane z projektem. Ustaw ten pakiet jako domyślny.

## Karty

Karta ma kolor (jeden spośród `{trefl, karo, kier, pik}`) i wartość (`{2..10,W,D,K,A}`).

Z warunków zadania wynika, ze potrzebujemy róznych sposobów wypisywania kolorów i wartości (`As kier` oraz `C A`), czyli prawdopodobnie przydadza się klasy `Kolor` i `Wartosc`. Ale możemy zacząć od napisów.

* zdefiniuj klasę `Karta` podobnie do `Osoba` w pierwszym tygodniu tak, aby

```
(Karta kolor: 'kier' wartosc: 'As' ) printString 
'As kier'
```

Wskazówki: spacja to `Character space`; konkatenacja napisów to przecinek: 
```
'Hello', Character space, 'world'
'Hello world'
```

Refleksja: kolory i wartości będziemy musieli sortować i losować, więc jednak bez klas się nie obejdzie.

* Zdefiniuj klasę `Kolor` z metodą klasową `kolory` która da w odpowiedzi wszystkie cztery kolory. Mozna zdefiniować poszczególne kolory jako podklasy.
* Przyda sie metoda `fromInteger`
* Będziemy porównywać kolory, więc `Kolor` moze byc podklasą `Magnitude` a w kazdym wypadku powinna mieć metody do porównywania. Sprawdź, które metody są niezbędne (oznaczone jako `subclassResponsibility` w klasie `Magnitude`).
* Przyjrzyj się klasie `Association`. Moze łatwiej zdefiniować  `Kolor`  jako podklasę `Association`?

```
Kolor kolory asSortedCollection
a SortedCollection('trefl' 'karo' 'kier' 'pik')

Kolor kolory asSortedCollection collect:  [ :k | k storeString ]
an OrderedCollection('Kolor nazwa:  ''trefl'' wartosc: 1' 'Kolor nazwa:  ''karo'' wartosc: 2' 'Kolor nazwa:  ''kier'' wartosc: 3' 'Kolor nazwa:  ''pik'' wartosc: 4')
```

W następnej kolejności zdefiuniujmy (abstrakcyjną) klasę `Wartosc` z dwoma podklasami: `Blotka` (wartosć liczbowa) i `Figura`. Każda z tych podklas bedzie miała własne metody wypisywania.

```
Figura new wartosc: 13
Krol

Blotka new wartosc: 6
6
```

(dziesiątka jest przypadkiem granicznym: ma wartosc liczbową, ale formalnie w brydżu nie jest blotką, zatem wedle upodoban moze trafic do jednej albo drugiej klasy)

W klasie `Wartosc` dodamy konstruktor `fromInteger: n`

```
Wartosc fromInteger: 12
Dama

Wartosc fromInteger: 7
7
```

Przyda się też metoda dająca wszystkie wartości:

```
Wartosc wszystkie collect: [ :w | w printString]
#('2' '3' '4' '5' '6' '7' '8' '9' '10' 'Walet' 'Dama' 'Krol' 'As')
```

`Wartosc` musi też implementować porównania. Mozna ją uczynic podklasą `Magnitude` (trzeba wtedy zdefiniować metodę `#hash`)

Teraz w klasie `Karta` tez warto dodać metodę `#wszystkie`:

```
Karta wszystkie asArray collect: [ :w | w printString] 
#('2 trefl' '3 trefl' '4 trefl' '5 trefl' '6 trefl' '7 trefl' '8 trefl' '9 trefl' '10 trefl' 'Walet trefl' 'Dama trefl' 'Krol trefl' 'As trefl' '2 karo' '3 karo' '4 karo' '5 karo' '6 karo' '7 karo' '8 karo' '9 karo' '10 karo' 'Walet karo' 'Dama karo' 'Krol karo' 'As karo' '2 kier' '3 kier' '4 kier' '5 kier' '6 kier' '7 kier' '8 kier' '9 kier' '10 kier' 'Walet kier' 'Dama kier' 'Krol kier' 'As kier' '2 pik' '3 pik' '4 pik' '5 pik' '6 pik' '7 pik' '8 pik' '9 pik' '10 pik' 'Walet pik' 'Dama pik' 'Krol pik' 'As pik')
```

Teraz będziemy potrafili wylosować kartę: wylosujemy indeks [1..52] i zajrzymy do powyzszej tablicy.
