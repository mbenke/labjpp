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
printOn: target
	"Append, to the <puttableStream>, target, a string whose characters are a 
	the same as those which would result from sending a #printString
	message to the receiver.
	N.B. This is really intended for development use. #displayOn: and #displayString
	are complementary methods for generating strings for presentation to an
	end-user."

	| name |
	name := self class name.
	target 
		nextPutAll: (name first isVowel ifTrue: ['an '] ifFalse: ['a ']);
		nextPutAll: name
```

Podobnie sprawa ma się z metodą `#displayString`:

```
displayString
	"Answer a String whose characters are a representation of the receiver as a user
	would want to see it.
	N.B. Subclasses should override #displayOn: to provide suitable implementations. 
	You may also want to override #displayString for performance reasons in certain 
	situations, because using a Stream can be a significant overhead where one 
	simply wants the display string of a single object. However, if you override
	#displayString you should also override #displayOn: to provide the same
	representation."

	| stream |
	stream := String writeStream: 32.
	self displayOn: stream.
	^stream contents


displayOn: aStream
	"Append, to aStream, a String whose characters are a representation of the receiver as a user
	would want to see it."

	self printOn: aStream
```

Strumienie jak mozna się spodziewać mogą służyć do pisania bądź czytania; są tez strumienie które umozliwiają jedno i drugie, ale trochę tu utrudnia sytuacje brak wielodziedziczenia - nie bedziemy się tu tym zajmować. Tytaj skupimy się na strumieniach do zapisu potrzebnych nam w naszym zadaniu. Na marginesie warto tylko wspomnieć, ze widzieliśmy już jedną podklasę `ReadStream` , mianowicie `Random`: strumień liczb losowych.


### Protokół `WriteStream`

W Smalltalku bardziej niż pozycja obiektu w hierarchii interesuje nas jego protokół - komunikaty na które odpowiada (i zachowanie w odpowiedzi na te komunkaty). Protokół `WriteStream` realizują też niezwiązane klasy, np. `StdioFileStream` czy `TranscriptShell`. Dobrym sposobem eksperymentowania ze strumieniami jest uzycie okna `Transcript`, np.

```
Transcript nextPut: $H; nextPutAll: 'ello'; cr
```
NB wypisze się dopiero po `cr` (a dokładniej po wywołanym przez nie `flush`).

* `nextPut: anObject` - wypisz `anObject` do strumienia. Ponieważ tu mamy do czynienia ze strumieniami znaków, w praktyce `anObject` musi byc znakiem.
* `nextPutAll: aCollection` - wypisz kolejne elementy kolekcji do strumienia; ponieważ napis jest kolekcja, możemy uzyc tego do wypisywania napisów.
* `flush` - opróznij bufor (nieistotne przy pisaniu do strumienia napisu, ale istotne np dla `Transcript`)
* `<< anObject` - wypisze znak albo kolekcję znaków. Przykład:
```
Transcript << $H << 'ello'; flush
```
* `space`, `tab`, `cr` - chyba jasne; `cr` wywołuje `flush`

Użyteczna sztuczka: `Transcript show` pokaze okno Transcript na wierzchu.

### Wracamy do zadania

Zaimplementuj `#displayOn:` i `printOn:` dla swoich klas (usuwając implementacje `#printString`)

```
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
```

Metody dla klas `Kolor`, `Wartosc` i `Karta` będa proste, trochę więcej pracy bedzie dla klasy `Reka`:

```
(Reka nazwa: $N karty: (Talia new tasuj wybierz: 13)) displayString
'(N)
P	W 8 7 6 4 
C	K D 10 7 6 4 
K	8 
T	6 
'
```

Ponieważ dotąd kolory i wartosci wyswietlalismy rosnąco, a teraz malejąco, przyda nam się tu metoda `#reverseDo:`

Kiedy mamy wyswietlanie ręki, wyświetlenie rozdania to prosta pętla `#do:separatedBy:`.
