### Zadanie 1

W poprzednim tygodniu pisaliśmy interpreter dla wyrażeń arytmetycznych:

``` haskell
    type Var = String
    data Exp = EInt Int
         | EOp  Op Exp Exp
         | EVar Var
         | ELet Var Exp Exp  -- let var = e1 in e2

    data Op = OpAdd | OpMul | OpSub
```

używając monady Reader dla środowiska.

Rozszerz ten interpreter o obsługę błędów w wypadku napotkania niezdefiniowanej zmiennej.

Porównaj rozwiązania używające róznych transformatorów:

``` haskell
type IM a = ReaderT Env (Either String) a
type IM a = ExceptT String (Reader Env) a
type IM a = ExceptT String ((->) Env) a
```
### Zadanie 2

Rozszerz interpreter języka Tiny (z poprzedniego tygodnia) o obsługę błędów (np. dzielenie).

Można użyć np. typu

``` haskell
type IM a = StateT IntState (Either String) a
```

gdzie `IntState` jest typem stanu interpretera.

### Zadanie 3

Rozszerz język Tiny i jego interpreter o deklaracje zmiennych lokalnych:

~~~
Stmt: S ::= ... | begin [D] S end
Decl: D ::= var x=e;
~~~

Tutaj dla obliczania `Stmt` najlepiej użyć jednocześnie monady `Reader` i `State`:
część Reader odczytuje funkcje z `Var` w nowy typ "lokacji pamięci" `Loc`
(wystarczy `type Loc=Int`), a część `State` operuje na mapowaniach
z `Loc` w `Int`. Trzeba też zaimplementować funkcję
`alloc :: (Map Loc Int) -> Loc`, która zwraca nieużywaną lokację.