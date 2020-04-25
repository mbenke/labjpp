## Zadanie 1

Zaimplementuj moduły StateTParser i SimpleParsers z wykładu

```
module StateTParser(Parser,runParser,item) where
import Control.Monad.State

type Parser a = StateT [Char] Maybe a
runParser :: Parser a -> [Char] -> Maybe (a, [Char])
runParser = -- użyj runStateT
item :: Parser Char
```

```
module SimpleParsers where

satisfy :: (Char->Bool) -> Parser Char
many, many1 :: Parser a -> Parser [a]
many p = ... -- użyj many1 i mplus lub <|>
many1 p = ... -- uzyj many i sekwencjonowania
pDigit, pNat :: Parser Int

test123 = runParser pNat "123 ala"
```

## Zadanie 2
Zaimplementuj moduł IdentityTrans dostarczający identycznościowego 
transformatora IdentityT:

    newtype IdentityT m a= IdentityT { runIdentity :: m a}
    instance (Functor m) => Functor (IdentityT m) where
    instance (Monad m) => Monad (IdentityT m) ...
    instance MonadTrans IdentityT ...
    instance MonadPlus m => MonadPlus (IdentityT m) ...

## Zadanie 3
 Zaimplementuj moduł `MaybeTrans` dostarczający transformatora MaybeT - podobnie jak wyżej, za wyjatkiem 

    instance MonadPlus (MaybeT m) ...

gdzie teraz dajemy wynik pierwszego z obliczeń, które zakończyło się sukcesem 
(`mplus (Just a) _ = Just a`)

## Zadanie 4

Zaimplementuj moduł StateTParser2 z wykładu:

~~~~
module StateTParser2(Parser,runParser,item) where
import Control.Monad.State
import MaybeTrans
import Control.Monad.Identity

-- Use the StateT transformer on MaybeT on Identity
type Parser a = StateT [Char] (MaybeT Identity) a

runParser :: Parser a -> [Char] -> Maybe (a,String)
item :: Parser Char
~~~~

 i przetestuj go z SimpleParsers (zastępując import StateTParser przez import StateTParser2)

~~~~
module SimpleParsers where
import Data.Char(isDigit,digitToInt)
import Control.Monad

import StateTParser2

pDigit1 :: Parser Int
pDigit1 = item >>= test where
  test x | isDigit x = return $ digitToInt x
         | otherwise = mzero

sat :: (Char->Bool) -> Parser Char
sat p = do {x <- item; if p x then return x else mzero}

char :: Char -> Parser Char
char x = sat (==x)

pDigit :: Parser Int
pDigit = fmap digitToInt $ sat isDigit

pDigits :: Parser [Int]
pDigits = many pDigit

pNat :: Parser Int 
-- pNat = pDigits >>= return . foldl (\x y -> 10*x+y) 0
pNat = fmap (foldl (\x y -> 10*x+y) 0) pDigits

many :: Parser a -> Parser [a]
many p = many1 p `mplus` return []

many1 p = do { a <- p; as <- many p; return (a:as)}

test123 = runParser pNat "123 ala"
~~~~
