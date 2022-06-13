# Smalltalk 3c 

## Strumienie

W klasie `Object` metoda `#printString` nie jest pierwotna; 
jest ona zaimplementowana przy pomocy `#printOn:`

```
printString
	"Answer a <readableString> whose characters are a description of the receiver 
	as a developer would want to see it."

	| stream |
	stream := String writeStream: 32.
	self printOn: stream.
	^stream contents
```

Metoda `#printOn:` wypisuje na strumień będący argumentem; 
w klasie `Object` wypisuje `an Object` (ew. nazwa klasy):

```
printOn: aStream
	"Append to the argument, aStream, a sequence of characters that  
	identifies the receiver."

	| title |
	title := self class name.
	aStream
		nextPutAll: (title first isVowel ifTrue: ['an '] ifFalse: ['a ']);
		nextPutAll: title
```

Podobnie sprawa ma się z metodą `#displayString`:

```
displayString
	"Answer a String whose characters are a representation of the receiver as a user
	would want to see it.
	
	| stream |
	stream := String writeStream: 32.
	self displayOn: stream.
	^stream contents


displayOn: aStream
	"Append, to aStream, a String whose characters are a representation of the receiver as a user
	would want to see it."

	self printOn: aStream
```

NB w Pharo zamiast `displayOn:` używamy  `displayStringOn:`

Strumienie jak mozna się spodziewać mogą służyć do pisania bądź czytania; są tez strumienie które umozliwiają jedno i drugie, ale trochę tu utrudnia sytuacje brak wielodziedziczenia - nie bedziemy się tu tym zajmować. Tytaj skupimy się na strumieniach do zapisu potrzebnych nam w naszym zadaniu. Na marginesie warto tylko wspomnieć, ze widzieliśmy już jedną podklasę `ReadStream` , mianowicie `Random`: strumień liczb losowych.


### Protokół `WriteStream`

W Smalltalku bardziej niż pozycja obiektu w hierarchii interesuje nas jego protokół - komunikaty na które odpowiada (i zachowanie w odpowiedzi na te komunkaty). Protokół `WriteStream` realizują też niezwiązane klasy, np. `StdioFileStream` czy `TranscriptShell`. Dobrym sposobem eksperymentowania ze strumieniami jest uzycie okna `Transcript`, np.

```
Transcript nextPut: $H; nextPutAll: 'ello'; cr; flush
```
NB wypisze się dopiero po `flush` (w Dolphinie `cr` wywołuje `flush`, w Pharo nie).

* `nextPut: anObject` - wypisz `anObject` do strumienia. Ponieważ tu mamy do czynienia ze strumieniami znaków, w praktyce `anObject` musi byc znakiem.
* `nextPutAll: aCollection` - wypisz kolejne elementy kolekcji do strumienia; ponieważ napis jest kolekcja, możemy uzyc tego do wypisywania napisów.
* `flush` - opróznij bufor (nieistotne przy pisaniu do strumienia napisu, ale istotne np dla `Transcript`)
* `<< anObject` - wypisze znak albo kolekcję znaków. Przykład:
```
Transcript << $H << 'ello'; flush
```
* `space`, `tab`, `cr` - chyba jasne; w Dolphinie `cr` wywołuje `flush`


## Wracamy do zadania

Zaimplementuj `displayStringOn:` i `printOn:` dla swoich klas (usuwając implementacje `printString`).

Teraz `Karta wszystkie` powinno dać

```
an OrderedCollection(2 trefl 3 trefl 4 trefl 5 trefl 6 trefl 7 trefl 8 trefl 9
trefl 10 trefl Walet trefl Dama trefl Król trefl As trefl 2 karo 3 karo 4 karo 5
karo 6 karo 7 karo 8 karo 9 karo 10 karo Walet karo Dama karo Król karo As karo
2 kier 3 kier 4 kier 5 kier 6 kier 7 kier 8 kier 9 kier 10 kier Walet kier Dama
kier Król kier As kier 2 pik 3 pik 4 pik 5 pik 6 pik 7 pik 8 pik 9 pik 10 pik
Walet pik Dama pik Król pik As pik)
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
]. talia
#('Krol trefl' '9 pik' 'Walet trefl' '6 pik' '7 karo' 'Walet karo' '3 pik' '8 kier' '2 pik' 'Krol karo' '10 kier' '2 trefl' 'Krol pik' 'As trefl' '9 kier' '5 kier' 'As pik' '5 karo' 'Walet kier' '4 pik' '8 pik' 'Dama trefl' '6 kier' '5 trefl' 'As kier' '2 kier' '3 trefl' '2 karo' '4 trefl' '10 trefl' 'Krol kier' '6 karo' 'Dama kier' 'Dama karo' '6 trefl' 'Dama pik' '9 trefl' '10 pik' '10 karo' '8 karo' '9 karo' '5 pik' 'As karo' '8 trefl' '4 kier' '7 pik' '7 kier' '3 karo' '4 karo' '7 trefl' 'Walet pik' '3 kier')
```

Umieśćmy teraz to wszystko w klasie `Talia` z metodami `#tasuj`, `#removeFirst` i innymi potrzebnymi (możnaby dziedziczyć po Collection, ale wtedy trzeba zaimplementować metody #add:, #do: i #remove:ifAbsent:).

```
Talia new
an OrderedCollection('2 trefl' '3 trefl' '4 trefl' '5 trefl' '6 trefl' '7 trefl' '8 trefl' '9 trefl' '10 trefl' 'Walet trefl' 'Dama trefl' 'Krol trefl' 'As trefl' '2 karo' '3 karo' '4 karo' '5 karo' '6 karo' '7 karo' '8 karo' '9 karo' '10 karo' 'Walet karo' 'Dama karo' 'Krol karo' 'As karo' '2 kier' '3 kier' '4 kier' '5 kier' '6 kier' '7 kier' '8 kier' '9 kier' '10 kier' 'Walet kier' 'Dama kier' 'Krol kier' 'As kier' '2 pik' '3 pik' '4 pik' '5 pik' '6 pik' '7 pik' '8 pik' '9 pik' '10 pik' 'Walet pik' 'Dama pik' 'Krol pik' 'As pik')

Talia new tasuj
an OrderedCollection('As kier' '8 trefl' '3 trefl' '9 karo' 'As pik' '9 kier' 'Krol pik' '7 karo' '5 trefl' 'Walet karo' '5 pik' '10 kier' '7 kier' '3 kier' '5 kier' '6 karo' '8 kier' '6 trefl' 'Dama karo' '10 trefl' '2 trefl' '7 trefl' '2 pik' '6 kier' '10 karo' '4 karo' '2 karo' 'As karo' '4 kier' '8 pik' '5 karo' 'Walet kier' '4 pik' '6 pik' 'Dama trefl' 'Walet pik' '2 kier' 'Krol kier' '4 trefl' 'Krol trefl' '7 pik' '10 pik' 'Krol karo' 'As trefl' 'Dama kier' '9 pik' '3 pik' '8 karo' 'Walet trefl' 'Dama pik' '3 karo' '9 trefl')
```

Użycie `OrderedCollection` zamiast `Array` pozwoli nam na rozdawanie przy użyciu `removeFirst`:

```
talia:= Talia new tasuj. 
r := OrderedCollection new. 13 timesRepeat:  [ r add: talia removeFirst ]. r
an OrderedCollection('2 trefl' '10 trefl' 'Dama karo' '8 trefl' 'Dama pik' '7 karo' 'Dama trefl' '7 trefl' '4 trefl' 'As pik' 'Walet kier' '5 karo' '5 kier')
```

(można dodać też metodę `#wybierz: n`):

```
(Talia new tasuj wybierz: 13) 
an OrderedCollection(Walet karo Dama trefl 2 kier 6 pik 8 karo 10 kier 2 pik 8 pik 3 karo Dama pik 7 pik 10 pik 4 trefl)
```

Ręka jest określona przez jej nazwę i karty. Warto użyć `SortedCollection` do przechowywania kart, zeby były odpowiednio ułożone w kolejności

```
(Reka nazwa: $N karty: (Talia new tasuj wybierz: 13) ) karty
a SortedCollection(3 trefl 4 trefl 6 trefl 9 trefl Dama trefl 2 karo 3 karo 8 karo 2 kier 4 kier 7 kier 10 kier Dama pik)
```

Przed użyciem `SortedCollection` warto upewnic sie, że, karty, kolory i wartości mają zdefiniowane metody porównania.

Teraz możemy już zdefiniować `Rozdanie` zawierające ręce N,E,S,W.

```
Rozdanie new printString
N(9 trefl, Dama trefl, Król trefl, 2 karo, As karo, 5 kier, 6 kier, 8 kier,
Walet kier, Król kier, 6 pik, 8 pik, As pik)
E(4 trefl, 6 trefl, 8 trefl, Walet trefl, 7 karo, Dama karo, 2 kier, 7 kier, 9
kier, As kier, 5 pik, 7 pik, 10 pik)
S(2 trefl, 3 trefl, As trefl, 3 karo, 5 karo, 6 karo, 8 karo, Walet karo, 3
kier, 2 pik, 3 pik, 9 pik, Walet pik)
W(5 trefl, 7 trefl, 10 trefl, 4 karo, 9 karo, 10 karo, Król karo, 4 kier, 10
kier, Dama kier, 4 pik, Dama pik, Król pik)
```

Do wypisywania ręki i rozdania może się przydać metoda `do: block1 separatedBy: block2`
