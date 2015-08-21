# Learn Prolog in x minutes

Prolog is a general purpose logic programming language associated with artificial intelligence and computational linguistics.

Prolog has its roots in first-order logic, a formal logic, and unlike many other programming languages, Prolog is declarative: the program logic is expressed in terms of relations, represented as facts and rules. A computation is initiated by running a query over these relations.

The language was first conceived by a group around Alain Colmerauer in Marseille, France, in the early 1970s and the first Prolog system was developed in 1972 by Colmerauer with Philippe Roussel.

## Installation

Here uses GNU Prolog as the compiler. It can be downloaded from [gprolog](http://www.gprolog.org/#download). 

## First Friend

`friends.pl`

```prolog
likes(cat, python).
likes(pig, python).
likes(mouse, ruby).

friend(X, Y) :- \+(X = Y), likes(X, Z), likes(Y, Z).
```

```sh
$ gprolog
| ?- ['friends.pl'].
compiling /Users/Jeoygin/Temp/trying/prolog/friends.pl for byte code...
/Users/Jeoygin/Temp/trying/prolog/friends.pl compiled, 5 lines read - 933 bytes written, 5 ms

(1 ms) yes
| ?- likes(cat, python).

yes
| ?- likes(pig, ruby).

no
| ?-
```

### Inferences and Variables

```
| ?- friend(pig, pig).

no
| ?- friend(cat, pig).

yes
| ?- friend(pig, cat).

yes
| ?- friend(cat, mouse).

no
| ?-
```

## Filling in the Blanks

`food.pl`

```prolog
food_type(velveeta, cheese).
food_type(ritz, cracker).
food_type(spam, meat).
food_type(sausage, meat).
food_type(jolt, soda).
food_type(twinkie, dessert).

flavor(sweet, dessert).
flavor(savory, meat).
flavor(savory, cheese).
flavor(sweet, soda).

food_flavor(X, Y) :- food_type(X, Z), flavor(Y, Z).
```

### Map Coloring

`map.pl`

```prolog
different(red, green). different(red, blue).
different(green, red). different(green, blue).
different(blue, red). different(blue, green).

coloring(Alabama, Mississippi, Georgia, Tennessee, Florida) :-
  different(Mississippi, Tennessee),
  different(Mississippi, Alabama),
  different(Alabama, Tennessee),
  different(Alabama, Mississippi),
  different(Alabama, Georgia),
  different(Alabama, Florida),
  different(Georgia, Florida),
  different(Georgia, Tennessee).
```

```
| ?- ['map.pl'].
compiling /Users/Jeoygin/Temp/trying/prolog/map.pl for byte code...
/Users/Jeoygin/Temp/trying/prolog/map.pl compiled, 13 lines read - 1732 bytes written, 7 ms

(1 ms) yes
| ?- coloring(Alabama, Mississippi, Georgia, Tennessee, Florida).

Alabama = blue
Florida = green
Georgia = red
Mississippi = red
Tennessee = green ?
```

### Unification, Part 1

`ohmy.pl`

```prolog
| ?- dorothy(lion, tiger, bear).

yes
| ?- dorothy(One, Two, Three).

One = lion
Three = bear
Two = tiger

yes
| ?- twin_cats(One, Two).

One = lion
Two = lion ? a

One = lion
Two = tiger

One = tiger
Two = lion

One = tiger
Two = tiger

yes
| ?-
```

## Recursion

`family.pl`

```prolog
| ?- ancestor(john, mike).

true ? ;

no
| ?- ancestor(tom, john).

true ?

yes
| ?- ancestor(tom, Who).

Who = john ? a

Who = mike

(1 ms) no
| ?- ancestor(Who, mike).

Who = john ? a

Who = tom

no
| ?-
```

## Lists and Tuples

### Unification, Part 2

```
| ?- (1, 2, 3) = (1, 2, 3).

yes
| ?- (1, 2, 3) = (1, 2, 3, 4).

no
| ?- (1, 2, 3) = (3, 2, 1).

no
| ?- (A, B, C) = (1, 2, 3).

A = 1
B = 2
C = 3

(1 ms) yes
| ?- (1, 2, 3) = (A, B, C).

A = 1
B = 2
C = 3

yes
| ?- (A, 2, C) = (1, B, 3).

A = 1
B = 2
C = 3

yes
| ?- [1, 2, 3] = [1, 2, 3].

yes
| ?- [1, 2, 3] = [X, Y, Z].

X = 1
Y = 2
Z = 3

yes
| ?- [2, 2, 3] = [X, X, Z].

X = 2
Z = 3

yes
| ?- [1, 2, 3] = [X, X, Z].

no
| ?- [] = [].

yes
| ?- [a, b, c] = [Head|Tail].

Head = a
Tail = [b,c]

(1 ms) yes
| ?- [] = [Head|Tail].

no
| ?- [a] = [Head|Tail].

Head = a
Tail = []

yes
| ?- [a, b, c] = [a|Tail].

Tail = [b,c]

yes
| ?- [a, b, c] = [a|[Head|Tail]].

Head = b
Tail = [c]

yes
| ?- [a, b, c, d, e] = [_, _|[Head|_]].

Head = c

yes
```

## Lists and Math

`list_math.pl`

```prolog
count(0, []).
count(Count, [Head|Tail]) :- count(TailCount, Tail), Count is TailCount + 1.

sum(0, []).
sum(Total, [Head|Tail]) :- sum(Sum, Tail), Total is Head + Sum.

average(Average, List) :- sum(Sum, List), count(Count, List), Average is Sum/Count.
```

```
| ?- count(What, [1]).

What = 1 ? ;

no
| ?- sum(What, [1, 2, 3]).

What = 6 ? ;

no
| ?- average(What, [1, 2, 3]).

What = 2.0 ? ;

no
```

## Using Rules in Both Directions

```
| ?- append([1], [2], [1, 2]).

yes
| ?- append([1], [2], [1, 3]).

no
| ?- append([1], [2], What).

What = [1,2]

yes
| ?- append([0], Who , [0, 1]).

Who = [1]

yes
| ?- append(One, Two, [1, 2]).

One = []
Two = [1,2] ? a

One = [1]
Two = [2]

One = [1,2]
Two = []

yes
```

`concat.pl`

```
concatenate([], List, List).
concatenate([Head|Tail1], List, [Head|Tail2]) :-
  concatenate(Tail1, List, Tail2).
```

```
| ?- concatenate([1, 2], [3], What).

What = [1,2,3]

yes
```

## Sudoku

`sudoku4.pl`

```
valid([]).
valid([Head|Tail]) :-
    fd_all_different(Head),
    valid(Tail).
sudoku(Puzzle, Solution) :-
        Solution = Puzzle,
        Puzzle = [S11, S12, S13, S14,
                  S21, S22, S23, S24,
                  S31, S32, S33, S34,
                  S41, S42, S43, S44],
        fd_domain(Solution, 1, 4),
        Row1 = [S11, S12, S13, S14],
        Row2 = [S21, S22, S23, S24],
        Row3 = [S31, S32, S33, S34],
        Row4 = [S41, S42, S43, S44],
        Col1 = [S11, S21, S31, S41],
        Col2 = [S12, S22, S32, S42],
        Col3 = [S13, S23, S33, S43],
        Col4 = [S14, S24, S34, S44],
        Square1 = [S11, S12, S21, S22],
        Square2 = [S13, S14, S23, S24],
        Square3 = [S31, S32, S41, S42],
        Square4 = [S33, S34, S43, S44],
        valid([Row1, Row2, Row3, Row4,
               Col1, Col2, Col3, Col4,
               Square1, Square2, Square3, Square4]).
```

```
| ?- sudoku([_, _, 2, 3, 
             _, _, _, _, 
             _, _, _, _, 
             3, 4, _, _], 
             Solution).

Solution = [4,1,2,3,2,3,4,1,1,2,3,4,3,4,1,2]

yes
```

## Eight Queens

`queens.pl`

```
valid_queen((Row, Col)) :-
    Range = [1,2,3,4,5,6,7,8],
    member(Row, Range), member(Col, Range).

valid_board([]).
valid_board([Head|Tail]) :- valid_queen(Head), valid_board(Tail).

rows([], []).
rows([(Row, _)|QueensTail], [Row|RowsTail]) :-
  rows(QueensTail, RowsTail).

cols([], []).
cols([(_, Col)|QueensTail], [Col|ColsTail]) :-
  cols(QueensTail, ColsTail).

diags1([], []).
diags1([(Row, Col)|QueensTail], [Diagonal|DiagonalsTail]) :-
  Diagonal is Col - Row,
  diags1(QueensTail, DiagonalsTail).

diags2([], []).
diags2([(Row, Col)|QueensTail], [Diagonal|DiagonalsTail]) :-
  Diagonal is Col + Row,
  diags2(QueensTail, DiagonalsTail).

eight_queens(Board) :- length(Board, 8), valid_board(Board),
  rows(Board, Rows),
  cols(Board, Cols),
  diags1(Board, Diags1),
  diags2(Board, Diags2),
  fd_all_different(Rows),
  fd_all_different(Cols),
  fd_all_different(Diags1),
  fd_all_different(Diags2).
```

```
| ?- eight_queens([(1, A), (2, B), (3, C), (4, D), (5, E), (6, F), (7, G), (8, H)]).

A = 1
B = 5
C = 8
D = 6
E = 3
F = 7
G = 2
H = 4 ?
```

`optimized_queens.pl`

```
valid_queen((Row, Col)) :- member(Col, [1,2,3,4,5,6,7,8]).

valid_board([]).
valid_board([Head|Tail]) :- valid_queen(Head), valid_board(Tail).

cols([], []).
cols([(_, Col)|QueensTail], [Col|ColsTail]) :-
  cols(QueensTail, ColsTail).

diags1([], []).
diags1([(Row, Col)|QueensTail], [Diagonal|DiagonalsTail]) :-
  Diagonal is Col - Row,
  diags1(QueensTail, DiagonalsTail).

diags2([], []).
diags2([(Row, Col)|QueensTail], [Diagonal|DiagonalsTail]) :-
  Diagonal is Col + Row,
  diags2(QueensTail, DiagonalsTail).

eight_queens(Board) :-
  Board = [(1, _), (2, _), (3, _), (4, _), (5, _), (6, _), (7, _), (8, _)],
  valid_board(Board),
  cols(Board, Cols),
  diags1(Board, Diags1),
  diags2(Board, Diags2),
  fd_all_different(Cols),
  fd_all_different(Diags1),
  fd_all_different(Diags2).
```

## Reference

* [Wikipedia: Prolog](http://en.wikipedia.org/wiki/Prolog)