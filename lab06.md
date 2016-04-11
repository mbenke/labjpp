1. Rozważmy gramatykę

~~~
F → ( L )
L → L E | ε
E → F | a
~~~

(a reprezentuje atom - identyfikator/literał/symbol)

Oblicz zbiory FIRST i FOLLOW dla wszytkich nietermionali oraz SELECT dla wszystkich produkcji. Czy gramatyka jest LL(1)? Czy mozna ją przekształcić do postaci LL(1) LR(0)?  SLR(1)?

Prześledź działanie automatu LR dla wybranego słowa z języka, np

~~~
( a ( ) a )
~~~

2.

~~~
F → ( L )
L → E L  | ε
E → F | a
~~~

Oblicz zbiory FIRST i FOLLOW dla wszytkich nietermionali oraz SELECT dla wszystkich produkcji. Czy gramatyka jest LL(1)? Czy mozna ją przekształcić do postaci LL(1) LR(0)?  SLR(1)?

Zaproponuj składnię abstrakcyjną i napisz parser top-down (przy użyciu kombinatorów parsujących bądź też metodą zejść rekurencyjnych) budujący drzewo struktury.

3. Stwórz automat LR dla gramatyki

~~~
E -> E + T | T
T -> T * F | F
F -> n | (E)
~~~
