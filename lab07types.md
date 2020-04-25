Napisz kontrolę typów dla rachunku lambda z prostymi typami.
Mozliwe rozszerzenia: let, unifikacja, rekonstrukcja typów,

Jako skladni abstrakcyjnej można uzyć na przykład

    module IntLambda where

    infixr 5 :->
    data Type = TInt | Type :-> Type 
      deriving Eq

    type Name = String
    data Exp = EInt Int | EVar Name 
	     | ELam Name Type Exp | EApp Exp Exp

    -- Przykładowe lambda-termy
    type Exp1 = Type -> Exp
    type Exp2 = Type -> Exp1
    type Exp3 = Type -> Exp2

    int :: Type
    int = TInt

    mkI :: Exp1
    mkI a = ELam "x" a $ EVar "x"

    mkK :: Exp2
    mkK a b = ELam "x" a $ ELam "y" b $ EVar "x"

    intK = mkK int int

    mkS :: Exp3
    mkS a b c = ELam "x" a $ ELam "y" b $ ELam "z" c
	      $ EApp 
		 (EApp (EVar "x") (EVar "z")) 
		 (EApp (EVar "y") (EVar "z")) 

    intS = mkS (int:->int:->int) (int:->int) int

    -- kombinator omega nie typuje sie w prostym rachunku lambda
    mkOmega :: Exp1
    mkOmega t = ELam "x" t $ EApp (EVar "x") (EVar "x")

    intOmega = mkOmega TInt


W pierwszej wersji mozna użyć po prostu error do raportowania błedów, 
potem jednak lepiej zrobić lepsze raportowanie, np z ErrorT

Przykładowa sesja:

```
    *Main> typeOf (EInt 42)
    Right int

    *Main> typeCheck intK
    int -> int -> int

    *Main> typeCheck intS
    (int -> int -> int) -> (int -> int) -> int -> int

    *Main> typeCheck intOmega
    Error: 
    Type error in
    \(x:int).x x
    In expression
    x x
    The type of x: int is not a function type

    *Main> typeCheck (EApp intS intK)
    (int -> int) -> int -> int

    *Main> typeCheck $ EApp (EApp intS intK) intK
    Error: 
    Type error in
    (\(x:int -> int -> int).\(y:int -> int).\(z:int).x z (y z)) (\(x:int).\(y:int).x) (\(x:int).\(y:int).x)
    For expression:
    \(x:int).\(y:int).x
    Cannot match expected type
    int -> int
    against inferred
    int -> int -> int
```

Z rekonstrukcją typów:

```
data Exp = EInt Int | EVar Name 
         | ELam Name Exp | EApp Exp Exp

-- Przykładowe lambda-termy


mkI :: Exp
mkI = ELam "x" $ EVar "x"

mkK :: Exp
mkK = ELam "x" $ ELam "y" $ EVar "x"


mkS :: Exp
mkS = ELam "x" $ ELam "y" $ ELam "z"
          $ EApp 
             (EApp (EVar "x") (EVar "z")) 
             (EApp (EVar "y") (EVar "z")) 

k7 :: Exp
k7 = EApp mkK (EInt 7)

-- kombinator omega nie typuje sie w prostym rachunku lambda
mkOmega :: Exp
mkOmega = ELam "x" $ EApp (EVar "x") (EVar "x")
```

Przykładowa sesja:

```
*CurryTypes> typeOf k7
Right (b -> int)

*CurryTypes> typeCheck $ EApp (EInt 7) (EInt 7)
Error: 
Type error in
7 7
types 'int' and 'int -> a' do not unify~

*CurryTypes> typeCheck mkOmega
Error: 
Type error in
\x->x x
Cannot construct the infinite type: "a" ~ a -> b
```

Z polimorficznym `let`:

```
data Exp = EInt Int | EVar Name
         | ELam Name Exp | EApp Exp Exp
         | ELet Name Exp Exp -- let x=e1 in e2

letOmega :: Exp
letOmega = ELet "x" (mkI' "z") (EApp (EVar "x") (EVar "x"))
```

```
*Main> letOmega
let x = \z->z in x x
*Main> typeCheck letOmega
c -> c
```


