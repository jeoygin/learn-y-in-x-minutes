# Learn Haskell in x minutes

Haskell is an advanced purely-functional programming language.

## Installation

You can download Haskell Platform, Compiler and base libraries from [Official Download](https://www.haskell.org/downloads).

For third party libraries, you can install using cabal:

```bash
$ cabal update
$ cabal install the-package
```

## Begin

```bash
$ ghci
GHCi, version 7.8.3: http://www.haskell.org/ghc/  :? for help
Loading package ghc-prim ... linking ... done.
Loading package integer-gmp ... linking ... done.
Loading package base ... linking ... done.
Prelude> 
```

## Expressions and Primitive Types

### Numbers

```haskell
Prelude> 1
1
Prelude> 1 + 2
3
Prelude> 1 + 2.0
3.0
Prelude> 1 + 2.0 * 3
7.0
Prelude> 1 * 2 + 3
5
Prelude> 1 * (2 + 3)
5
Prelude> 2 * (3 + 4)
14
```

### Character Data

```haskell
Prelude> "hello"
"hello"
Prelude> "hello" + " world"

<interactive>:12:9:
    No instance for (Num [Char]) arising from a use of ‘+’
    In the expression: "hello" + " world"
    In an equation for ‘it’: it = "hello" + " world"
Prelude> "hello" ++ " world"
"hello world"
Prelude> 'a'
'a'
Prelude> ['a', 'b']
"ab"
```

### Booleans

```haskell
Prelude> (4 + 5) == 9
True
Prelude> (5 + 5) /= 10
False
Prelude> if (5 == 5) ten "true"

<interactive>:18:23:
    parse error (possibly incorrect indentation or mismatched brackets)
Prelude> if (5 == 5) then "true" else "false"
"true"
Prelude> if 1 then "true" else "false"

<interactive>:2:4:
    No instance for (Num Bool) arising from the literal ‘1’
    In the expression: 1
    In the expression: if 1 then "true" else "false"
    In an equation for ‘it’: it = if 1 then "true" else "false"
```

### Strong Typed

```haskell
Prelude> "one" + 1

<interactive>:7:7:
    No instance for (Num [Char]) arising from a use of ‘+’
    In the expression: "one" + 1
    In an equation for ‘it’: it = "one" + 1
Prelude> :set +t
Prelude> 5
5
it :: Num a => a
Prelude> 5.0
5.0
it :: Fractional a => a
Prelude> "hello"
"hello"
it :: [Char]
Prelude> (5 == (2 + 3))
True
it :: Bool
Prelude> :t 5
5 :: Num a => a
```

## Functions

### Defining Basic Functions

```haskell
Prelude> let x = 10
Prelude> x
10
Prelude> let double x = x * 2
Prelude> double 2
4
```

`double.hs`

```haskell
module Main where
    double x = x + x
```

```haskell
Prelude> :load double.hs
[1 of 1] Compiling Main             ( double.hs, interpreted )
Ok, modules loaded: Main.
*Main> double 5
10
```

`double_with_type.hs`

```haskell
module Main where

    double :: Integer -> Integer
    double x = x + x
```

```haskell
Prelude> :load double_with_type.hs 
[1 of 1] Compiling Main             ( double_with_type.hs, interpreted )
Ok, modules loaded: Main.
*Main> double 5
10
*Main> :t double
double :: Integer -> Integer
```

### Recrusion

```haskell
Prelude> let fact x = if x == 0 then 1 else fact (x - 1) * x
Prelude> fact 3
6
```

`factorial.hs`

```haskell
module Main where
    factorial :: Integer -> Integer
    factorial 0 = 1
    factorial x = x * factorial (x - 1)
```

`fact_with_guard.hs`

```haskell
module Main where
    factorial :: Integer -> Integer
    factorial x
        | x > 1 = x * factorial (x - 1)
        | otherwise = 1
```

## Tuples and Lists

`fib.hs`

```haskell
module Main where
    fib :: Integer -> Integer
    fib 0 = 1
    fib 1 = 1
    fib x = fib (x - 1) + fib (x - 2)
```

### Programming with Tuples

`fib_tuple.hs`

```haskell
module Main where
    fibTuple :: (Integer, Integer, Integer) -> (Integer, Integer, Integer)
    fibTuple (x, y, 0) = (x, y, 0)
    fibTuple (x, y, index) = fibTuple (y, x + y, index - 1)
    
    fibResult :: (Integer, Integer, Integer) -> Integer
    fibResult (x, y, z) = x
    
    fib :: Integer -> Integer
    fib x = fibResult (fibTuple (0, 1, x))
```

```haskell
Prelude> :load fib_tuple.hs 
[1 of 1] Compiling Main             ( fib_tuple.hs, interpreted )
Ok, modules loaded: Main.
*Main> fibTuple(0, 1, 4)
(3,5,0)
*Main> fib 100
354224848179261915075
*Main> fib 1000
43466557686937456435688527675040625802564660517371780
40248172908953655541794905189040387984007925516929592
25930803226347752096896232398733224711616429964409065
33187938298969649928516003704476137795166849228875
```

### Using Tuples and Composition

```haskell
*Main> let second = head . tail
*Main> second [1, 2]
2
*Main> second [3, 4, 5]
4
```

`fib_pair.hs`

```haskell
module Main where
    fibNextPair :: (Integer, Integer) -> (Integer, Integer)
    fibNextPair (x, y) = (y, x + y)
    
	
    fibNthPair :: Integer -> (Integer, Integer)
    fibNthPair 1 = (1, 1)
    fibNthPair n = fibNextPair (fibNthPair (n - 1))
    
    fib :: Integer -> Integer
    fib = fst . fibNthPair
```

```haskell
*Main> :load fib_pair.hs 
[1 of 1] Compiling Main             ( fib_pair.hs, interpreted )
Ok, modules loaded: Main.
*Main> fibNthPair(8)
(21,34)
*Main> fibNthPair(9)
(34,55)
*Main> fibNthPair(10)
(55,89)
*Main> fib(8)
21
```

### Traversing Lists

`lists.hs`

```haskell
module Main where
    size [] = 0
    size (h:t) = 1 + size t
    
    prod [] = 1
    prod (h:t) = h * prod t
```

```haskell
Prelude> let (h:t) = [1, 2, 3, 4]
Prelude> h
1
Prelude> t
[2,3,4]
Prelude> :load lists.hs
[1 of 1] Compiling Main             ( lists.hs, interpreted )
Ok, modules loaded: Main.
*Main> size "Amazing."
8
*Main> zip "cat" "dog"
[('c','d'),('a','o'),('t','g')]
*Main> zip "cat" "tiger"
[('c','t'),('a','i'),('t','g')]
*Main> zip ["cat", "dog"] ["tiger", "lion"]
[("cat","tiger"),("dog","lion")]
```

## Generating Lists

### Recursion

```haskell
*Main> let h:t = [1, 2, 3]
*Main> h
1
*Main> t
[2,3]
*Main> 1:[2, 3]
[1,2,3]
*Main> [1]:[2, 3]

<interactive>:14:1:
    No instance for (Num [t0]) arising from a use of ‘it’
    In a stmt of an interactive GHCi command: print it
*Main> [1]:[[2], [3, 4]]
[[1],[2],[3,4]]
*Main> [1]:[]
[[1]]
```

`all_even.hs`

```haskell
module Main where
    allEven :: [Integer] -> [Integer]
    allEven [] = []
    allEven (h:t) = if even h then h:allEven t else allEven t
```

### Ranges and Composition

```haskell
Prelude> [1..2]
[1,2]
Prelude> [1..4]
[1,2,3,4]
Prelude> [10..4]
[]
Prelude> [10, 8 .. 4]
[10,8,6,4]
Prelude> [10, 9.5 .. 4]
[10.0,9.5,9.0,8.5,8.0,7.5,7.0,6.5,6.0,5.5,5.0,4.5,4.0]
Prelude> take 5 [1..]
[1,2,3,4,5]
Prelude> take 5 [0, 2 ..]
[0,2,4,6,8]
```

### List Comprehensions

```haskell
Prelude> [x * 2 | x <- [1, 2, 3]]
[2,4,6]
Prelude> [ (y, x) | (x, y) <- [(1, 2), (2, 3), (3, 4)]]
[(2,1),(3,2),(4,3)]
Prelude> [ (4 - x, y) | (x, y) <- [(1, 2), (2, 3), (3, 4)]]
[(3,2),(2,3),(1,4)]
Prelude> let colors = ["red", "green", "blue"]
Prelude> [(a, b) | a <- colors, b <- colors]
[("red","red"),("red","green"),("red","blue"),("green","red"),("green","green"),("green","blue"),("blue","red"),("blue","green"),("blue","blue")]
Prelude> [(a, b) | a <- colors, b <- colors, a /= b]
[("red","green"),("red","blue"),("green","red"),("green","blue"),("blue","red"),("blue","green")]
Prelude> [(a, b) | a <- colors, b <- colors, a < b]
[("green","red"),("blue","red"),("blue","green")]
```

## Higher-Order Functions

### Anonymous Functions

```haskell
Loading package base ... linking ... done.
Prelude> (\x -> x) "Logical."
"Logical."
Prelude> (\x -> x ++ " captian.") "Logical,"
"Logical, captian."
```

### map and where

```haskell
Prelude> map (\x -> x * x) [1, 2, 3]
[1,4,9]
```

`map.hs`

```haskell
module Main where
    squareAll list = map square list
        where square x = x * x
```

```haskell
Prelude> :load map.hs 
[1 of 1] Compiling Main             ( map.hs, interpreted )
Ok, modules loaded: Main.
*Main> squareAll [1, 2, 3]
[1,4,9]
*Main> map (+ 1) [1, 2, 3]
[2,3,4]
```

### filter, foldl, foldr

```haskell
Prelude> odd 5
True
Prelude> filter odd [1, 2, 3, 4, 5]
[1,3,5]
Prelude> foldl (\x carryOver -> carryOver + x) 0 [1 .. 10]
55
Prelude> foldl1 (+) [1 .. 3]
6
Prelude> 1 + 2 + 3
6
Prelude>
```

## Partially Applied Functions and Currying

```haskell
Prelude> let prod x y = x * y
Prelude> prod 3 4
12
Prelude> :t prod
prod :: Num a => a -> a -> a
Prelude> let double = prod 2
Prelude> let triple = prod 3
Prelude> double 3
6
Prelude> triple 4
12
```

## Lazy Evaluation

`my_range.hs`

```haskell
module Main where
    myRange start step = start:(myRange (start + step) step)
```

```haskell
Prelude> :load my_range.hs 
[1 of 1] Compiling Main             ( my_range.hs, interpreted )
Ok, modules loaded: Main.
*Main> take 10 (myRange 10 1)
[10,11,12,13,14,15,16,17,18,19]
*Main> take 5 (myRange 0 5)
[0,5,10,15,20]
```

`lazy_fib.hs`

```haskell
module Main where
    lazyFib x y = x:(lazyFib y (x + y))

    fib = lazyFib 1 1

    fibNth x = head (drop (x - 1) (take (x) fib))
```

```haskell
*Main> :load lazy_fib.hs 
[1 of 1] Compiling Main             ( lazy_fib.hs, interpreted )
Ok, modules loaded: Main.
*Main> take 5 (lazyFib 0 1)
[0,1,1,2,3]
*Main> take 5 (fib)
[1,1,2,3,5]
*Main> take 5 (drop 20 (lazyFib 0 1))
[6765,10946,17711,28657,46368]
*Main> fibNth 3
2
*Main> fibNth 6
8
*Main> take 5 (zipWith (+) fib (drop 1 fib))
[2,3,5,8,13]
*Main> take 5 (map (*2) [1 ..])
[2,4,6,8,10]
*Main> take 5 (map ((* 2) . (* 5)) fib)
[10,10,20,30,50]
```

## Classes and Types

### Basic Types

```haskell
Prelude> :set +t
Prelude> 'c'
'c'
it :: Char
Prelude> "hello"
"hello"
it :: [Char]
Prelude> ['a', 'b', 'c']
"abc"
it :: [Char]
Prelude> "hello" == ['h', 'e', 'l', 'l', 'o']
True
it :: Bool
Prelude> True
True
it :: Bool
Prelude> False
False
it :: Bool
```

### User-Defined Types

`cards.hs`

```haskell
module Main where
    data Suit = Spades | Hearts
    data Rank = Ten | Jack | Queen | King | Ace
```

`cards-with-show.hs`

```haskell
module Main where
    data Suit = Spades | Hearts deriving (Show)
    data Rank = Ten | Jack | Queen | King | Ace deriving (Show)
    type Card = (Rank, Suit)
    type Hand = [Card]

    value :: Rank -> Integer
    value Ten = 1
    value Jack = 2
    value Queen = 3
    value King = 4
    value Ace = 5

    cardValue :: Card -> Integer
    cardValue (rank, suit) = value rank
```

```haskell
Prelude> :load cards-with-show.hs 
[1 of 1] Compiling Main             ( cards-with-show.hs, interpreted )
Ok, modules loaded: Main.
*Main> cardValue (Ten, Hearts)
1
```

### Functions and Polymorphism

`triplet.hs`

```haskell
module Main where
    data Triplet a = Trio a a a deriving (Show)
```

```haskell
*Main> :load triplet.hs 
[1 of 1] Compiling Main             ( triplet.hs, interpreted )
Ok, modules loaded: Main.
*Main> :t Trio 'a' 'b' 'c'
Trio 'a' 'b' 'c' :: Triplet Cha
```

### Recursive Types

`tree.hs`

```haskell
module Main where
    data Tree a = Children [Tree a] | Leaf a deriving (Show)
    
    depth (Leaf _) = 1
    depth (Children c) = 1 + maximum (map depth c)
```

```haskell
Prelude> :load tree.hs 
[1 of 1] Compiling Main             ( tree.hs, interpreted )
Ok, modules loaded: Main.
*Main> let leaf = Leaf 1
*Main> leaf
Leaf 1
*Main> let (Leaf value) = leaf
*Main> value
1
*Main> Children[Leaf 1, Leaf 2]
Children [Leaf 1,Leaf 2]
*Main> let tree = Children[Leaf 1, Children [Leaf 2, Leaf 3]]
*Main> tree
Children [Leaf 1,Children [Leaf 2,Leaf 3]]
*Main> let (Children ch) = tree
*Main> ch
[Leaf 1,Children [Leaf 2,Leaf 3]]
*Main> let (fst:tail) = ch
*Main> fst
Leaf 1
```

## Monads

### The Problem: Drunken Pirate

`drunken-pirate.hs`

```
module Main where

    stagger :: (Num t) => t -> t
    stagger d = d + 2
    crawl d = d + 1

    treasureMap d = 
        crawl (
        stagger (
        stagger d))

    letTreasureMap (v, d) = let d1 = stagger d
                                d2 = stagger d1
                                d3 = crawl d2
                            in d3
```

### Building a Monad from Scratch

`drunken-monad.hs`

```haskell
module Main where
    data Position t = Position t deriving (Show)

    stagger (Position d) = Position (d + 2)
    crawl (Position d) = Position (d + 1)

    rtn x = x
    x >>== f = f x

    treasureMap pos = pos >>== 
                      stagger >>== 
                      stagger >>== 
                      crawl >>== 
                      rtn
```

```haskell
Prelude> :load drunken-monad.hs 
[1 of 1] Compiling Main             ( drunken-monad.hs, interpreted )
Ok, modules loaded: Main.
*Main> treasureMap (Position 0)
Position 5
*Main>
```

### Monads and do Notation

`io.hs`

```haskell
module Main where
    tryIo = do  putStr "Enter your name: " ;
                line <- getLine ;
                let { backwards = reverse line } ;
                return ("Hello. Your name backwards is " ++ backwards)
```

```haskell
*Main> :load io.hs 
[1 of 1] Compiling Main             ( io.hs, interpreted )
Ok, modules loaded: Main.
*Main> tryIo 
Enter your name: Jeoygin
"Hello. Your name backwards is nigyoeJ"
```

### Different Computational Strategies

```haskell
instance Monad [] where
    m >>= f = concatMap f m
    return x = [x]
```

```haskell
*Main> let cartesian (xs, ys) = do x <- xs; y <- ys; return (x, y)
*Main> cartesian ([1..2], [3..4])
[(1,3),(1,4),(2,3),(2,4)]
```

`password.hs`

```haskell
module Main where
    crack = do x <- ['a'..'c'] ; y <- ['a'..'c'] ; z <- ['a'..'c'] ; 
               let { password = [x, y, z] } ;
               if attempt password
                    then return (password, True)
                    else return (password, False)
    
    attempt pw = if pw == "cab" then True else False
```

```haskell
*Main> :load password.hs 
[1 of 1] Compiling Main             ( password.hs, interpreted )
Ok, modules loaded: Main.
*Main> crack 
[("aaa",False),("aab",False),("aac",False),("aba",False),("abb",False),("abc",False),("aca",False),("acb",False),("acc",False),("baa",False),("bab",False),("bac",False),("bba",False),("bbb",False),("bbc",False),("bca",False),("bcb",False),("bcc",False),("caa",False),("cab",True),("cac",False),("cba",False),("cbb",False),("cbc",False),("cca",False),("ccb",False),("ccc",False)]
```

### Maybe Monad

```haskell
Prelude> Just "some string"
Just "some string"
Prelude> Just Nothing
Just Nothing
```

```haskell
case (html doc) of
  Nothing -> Nothing
  Just x -> case body x of
              Nothing -> Nothing
              Just y -> paragraph 2 y
```

```haskell
data Maybe a = Nothing | Just a

instance Monad Maybe where
    return         = Just
    Nothing  >>= f = Nothing
    (Just x) >>= f = f x

Just someWebPage >>= html >>= body >>= paragraph >>= return
```

## Reference

* [Haskell Language](https://www.haskell.org/)
* [Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages](http://www.amazon.com/Seven-Languages-Weeks-Programming-Programmers/dp/193435659X)
