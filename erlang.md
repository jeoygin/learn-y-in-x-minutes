# Learn Erlang in x minutes

[TOC]

**What is Erlang?**

Erlang is a programming language used to build massively scalable soft real-time systems with requirements on high availability. Some of its uses are in telecoms, banking, e-commerce, computer telephony and instant messaging. Erlang's runtime system has built-in support for concurrency, distribution and fault tolerance.

**What is OTP?**

OTP is set of Erlang libraries and design principles providing middle-ware to develop these systems. It includes its own distributed database, applications to interface towards other languages, debugging and release handling tools.

## Installation

Erlang/OTP can be downloaded from [erlang-solutions](https://www.erlang-solutions.com/downloads/download-erlang-otp).

## Basis

### Coments, Variables and Expressions

```erlang
$ erl
1> % This is a comment
1> 1 + 2.
3
2> 1 + 2.0.
3.0
3> "Hello world".
"Hello world"
4> [1, 2, 3].
[1,2,3]
5> [72, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100].
"Hello world"
6> 1 + "2".
** exception error: an error occurred when evaluating an arithmetic expression
     in operator  +/2
        called as 1 + "2"
7> var = 0.
** exception error: no match of right hand side value 0
8> Var = 0.
0
9> Var = 1.
** exception error: no match of right hand side value 1
10> Var.
0
```

### Atoms, Lists and Tuples

```erlang
11> red.
red
12> Color = blue.
blue
13> Color.
blue
14> [0, 1, 2].
[0,1,2]
15> [0, 1, "two"].
[0,1,"two"]
16> List = [0, 1, 2].
[0,1,2]
17> {zero, one, two}.
{zero,one,two}
18> Origin = {0, 0}.
{0,0}
19> {name, "Tom"}.
{name,"Tom"}
20> {person, {name, "Tom"}, {country, "China"}}.
{person,{name,"Tom"},{country,"China"}}
```

### Pattern Matching

```erlang
21> Person = {person, {name, "Tom"}, {country, "China"}}.
{person,{name,"Tom"},{country,"China"}}
22> {person, {name, Name}, {country, Country}} = Person. 
{person,{name,"Tom"},{country,"China"}}
23> Name.
"Tom"
24> Country.
"China"
25> [Head | Tail] = [0, 1, 2].
[0,1,2]
26> Head.
0
27> Tail.
[1,2]
28> [Zero, One | Rest] = [0, 1, 2].
[0,1,2]
29> Zero.
0
30> One.
1
31> Rest.
[2]
32> [X | Rest] = [].
** exception error: no match of right hand side value []
33> one = 1.
** exception error: no match of right hand side value 1
```

### Bit Matching

```erlang
34> W = 0.
0
35> X = 1.
1
36> Y = 2.
2
37> Z = 3.
3
38> All = <<W:3, X:3, Y:5, Z:5>>.
<<4,67>>
39> <<A:3, B:3, C:5, D:5>> = All.
<<4,67>>
40> A.
0
41> B.
1
42> C.
2
43> D.
3
```

### Functions

`basic.erl`

```erlang
-module(basic).
-export([mirror/1]).

mirror(Anything) -> Anything.
```

```
44> c(basic).
{ok,basic}
45> mirror(hello). 
** exception error: undefined shell command mirror/1
46> basic:mirror(hello).
hello
47> basic:mirror(0).
0
```

`matching_function.erl`

```erlang
-module(matching_function).
- export([number/1]).

number(zero)   -> 0;
number(one)    -> 1;
number(two)    -> 2.
```

```erlang
49> c(matching_function).
{ok,matching_function}
50> matching_function:number(zero).
0
51> matching_function:number(one).
1
52> matching_function:number(two).
2
53> matching_function:number(three).
** exception error: no function clause matching 
                    matching_function:number(three) (matching_function.erl, line 4)
```

`yet_again.erl`

```erlang
-module(yet_again).
-export([another_factorial/1]).
-export([another_fib/1]).

another_factorial(0) -> 1;
another_factorial(N) -> N * another_factorial(N-1).

another_fib(0) -> 1;
another_fib(1) -> 1;
another_fib(N) -> another_fib(N-1) + another_fib(N-2).
```

```erlang
55> c(yet_again).
{ok,yet_again}
56> yet_again:another_factorial(5).
120
57> yet_again:another_factorial(20).
2432902008176640000
58> yet_again:another_factorial(200).
788657867364790503552363213932185062295135977687173263294742533244359449963403342920304284011984623904177212138919638830257642790242637105061926624952829931113462857270763317237396988943922445621451664240254033291864131227428294853277524242407573903240321257405579568660226031904170324062351700858796178922222789623703897374720000000000000000000000000000000000000000000000000
```

## Control Structures

### Case

```erlang
1> Color = "red".
"red"
2> case Color of
2>     "red" -> bloodred;
2>     "blue" -> apecblue
2> end.
bloodred
3> case Color of
3>     "green" -> green;    
3>     _ -> others
3> end.
others
```

### If

```erlang
4> X = 0.
0
5> if 
5>   X > 0 -> positive;
5>   X < 0 -> negative
5> end
5> .
** exception error: no true branch found when evaluating an if expression
6> if                 
6>   X > 0 -> positive;
6>   X < 0 -> negative;
6>   true  -> zero
6> end.
zero
```

### Anonymous Functions

```erlang
7> Negate = fun(I) -> -I end.
#Fun<erl_eval.6.80484245>
8> Negate(1).
-1
9> Negate(-1).
1
```

## Lists and Higher-Order Functions

### Applying Functions to Lists

```erlang
10> Numbers = [1, 2, 3, 4].
[1,2,3,4]
11> lists:foreach(fun(Number) -> io:format("~p~n", [Number]) end, Numbers).
1
2
3
4
ok
12> Print = fun(X) -> io:format("~p~n", [X]) end.
#Fun<erl_eval.6.80484245>
13> lists:foreach(Print, Numbers).
1
2
3
4
ok
14> lists:map(fun(X) -> X + 1 end, Numbers).
[2,3,4,5]
15> Small = fun(X) -> X < 3 end.
#Fun<erl_eval.6.80484245>
16> Small(4).
false
17> Small(1).
true
18> lists:filter(Small, Numbers).
[1,2]
19> lists:all(Small, [0, 1, 2]).
true
20> lists:all(Small, [0, 1, 2, 3]).
false
21> lists:any(Small, [0, 1, 2, 3]).
true
22> lists:any(Small, [3, 4, 5]).
false
23> lists:any(Small, []).
false
24> lists:all(Small, []).
true
25> lists:takewhile(Small, Numbers).
[1,2]
26> lists:dropwhile(Small, Numbers).
[3,4]
27> lists:takewhile(Small, [1, 2, 1, 4, 1]).
[1,2,1]
28> lists:dropwhile(Small, [1, 2, 1, 4, 1]).
[4,1]
```

### foldl

```erlang
29> Numbers.
[1,2,3,4]
30> lists:foldl(fun(X, Sum) -> X + Sum end, 0, Numbers).
10
31> Adder = fun(ListItem, SumSoFar) -> ListItem + SumSoFar end.
#Fun<erl_eval.12.80484245>
32> InitialSum = 0.
0
33> lists:foldl(Adder, InitialSum, Numbers).
10
```

## Advanced List Concepts

### List Construction

`double.erl`

```erlang
-module(double).
-export([double_all/1]).

double_all([]) -> [];
double_all([First|Rest]) -> [First+First|double_all(Rest)].
```

```erlang
1> c(double).
{ok,double}
2> double:double_all([1, 2, 3]).
[2,4,6]
3> [1 | [2, 3]].
[1,2,3]
4> [[2, 3] | 1].
[[2,3]|1]
5> [[] | [2, 3]].
[[],2,3]
6> [1 | []].
[1]
```

### List Comprehensions

```erlang
1> Fibs = [1, 1, 2, 3, 5].
[1,1,2,3,5]
2> Double = fun(X) -> X * 2 end.
#Fun<erl_eval.6.80484245>
3> lists:map(Double, Fibs).
[2,2,4,6,10]
4> [Double(X) || X <- Fibs].
[2,2,4,6,10]
5> [X * 2 || X <- [1, 1, 2, 3, 5]].
[2,2,4,6,10]
6> Cart = [{pencil, 4, 0.25}, {pen, 1, 1.20}, {paper, 2, 0.20}].
[{pencil,4,0.25},{pen,1,1.2},{paper,2,0.2}]
7> WithTax = [{Product, Quantity, Price, Price * Quantity * 0.08} ||
7>   {Product, Quantity, Price} <- Cart].
[{pencil,4,0.25,0.08},{pen,1,1.2,0.096},{paper,2,0.2,0.032}]
8> Cat = [{Product, Price} || {Product, _, Price} <- Cart].
[{pencil,0.25},{pen,1.2},{paper,0.2}]
9> DiscountedCat = [{Product, Price / 2} || {Product, Price} <- Cat].
[{pencil,0.125},{pen,0.6},{paper,0.1}]
10> [X || X <- [1, 2, 3, 4], X < 4, X > 1].
[2,3]
11> [{X, Y} || X <- [1, 2, 3, 4], X < 3, Y <- [5, 6]].
[{1,5},{1,6},{2,5},{2,6}]

```

## Basic Concurrency Primitives

### A Basic Receive Loop

`translate.erl`

```erlang
-module(translate).
-export([loop/0]).

loop() ->
    receive
        "tiger" ->
            io:format("animal~n"),
            loop();
        "red" ->
            io:format("color~n"),
            loop();

        _ ->
            io:format("I don't understand.~n"),
            loop()
    end.
```

### Spawning a Process

```erlang
1> c(translate).
{ok,translate}
2> Pid = spawn(fun translate:loop/0).
<0.40.0>
```

### Sending Messages

```erlang
3> Pid ! "tiger".
animal
"tiger"
4> Pid ! "red".  
color
"red"
5> Pid ! "blabla".
I don't understand.
"blabla"
```

## Synchronous Messaging

### Receiving Synchronously

```erlang
receive
    {Pid, "tiger"} ->
        Pid ! "animal",
        loop(),
        ...
```

### Sending Synchronously

```erlang
Receiver ! "message_to_translate",
    receive
        Message -> do_something_with(Message)
    end
```

```erlang
translate(To, Word) ->
    To ! {self(), Word},
    receive
        Translation -> Translation
    end.
```

`translate_service.erl`

```erlang
-module(translate_service).
-export([loop/0, translate/2]).

loop() ->
  receive
    {From, "tiger"} ->
      From ! "animal",
      loop();

    {From, "red"} ->
      From ! "color",
      loop();

    {From, _} ->
      From ! "I don't understand.",
      loop()
  end.

translate(To, Word) ->
  To ! {self(), Word},
  receive
    Translation -> Translation
  end.
```

```erlang
1> c(translate_service).
{ok,translate_service}
2> Translator = spawn(fun translate_service:loop/0).
<0.40.0>
3> translate_service:translate(Translator, "tiger").
"animal"
4> translate_service:translate(Translator, "red").
"color"
```

## Linking Processes for Reliability

### Spawning a Linked Process

`roulette.erl`

```erlang
-module(roulette).
-export([loop/0]).

loop() ->
  receive
    3 -> io:format("bang.~n"), exit({roulette, die, at, erlang:time()});
    _ -> io:format("click~n"), loop()
  end.

```

```erlang
1> c(roulette).
{ok,roulette}
2> Gun = spawn(fun roulette:loop/0).
<0.40.0>
3> Gun ! 1.
click
1
4> Gun ! 3.
bang.
3
5> Gun ! 4.
4
6> Gun ! 1.
1
7> erlang:is_process_alive(Gun).
false
```

### Cornoer

`coroner.erl`

```erlang
-module(coroner).
-export([loop/0]).

loop() ->
  process_flag(trap_exit, true),
  receive
    {monitor, Process} ->
      link(Process),
      io:format("Monitoring process.~n"),
      loop();

    {'EXIT', From, Reason} ->
      io:format("The shooter ~p died with reason ~p.", [From, Reason]),
      io:format("Start another one.~n"),
      loop()
  end.
```

```erlang
1> c(roulette).
{ok,roulette}
2> c(coroner).
{ok,coroner}
3> Revolver = spawn(fun roulette:loop/0).
<0.46.0>
4> Coroner = spawn(fun coroner:loop/0).
<0.48.0>
5> Coroner ! {monitor, Revolver}.
Monitoring process.
{monitor,<0.46.0>}
6> Revolver ! 1.
click
1
7> Revolver ! 3.
bang.
The shooter <0.46.0> died with reason {roulette,die,at,{18,43,1}}.3
Start another one.
```

### From Coroner to Doctor

`doctor.erl`

```erlang
-module(doctor).
-export([loop/0]).

loop() ->
  process_flag(trap_exit, true),
  receive
    new ->
      io:format("Creating and monitoring process.~n"),
      register(revolver, spawn_link(fun roulette:loop/0)),
      loop();

    {'EXIT', From, Reason} ->
      io:format("The shooter ~p died with reason ~p.", [From, Reason]),
      io:format(" Restarting. ~n "),
      self() ! new,
      loop()
  end.
```

```erlang
1> c(roulette).
{ok,roulette}
2> c(doctor).
{ok,doctor}
3> Doc = spawn(fun doctor:loop/0).
<0.46.0>
4> revolver ! 1.
** exception error: bad argument
     in operator  !/2
        called as revolver ! 1
5> Doc ! new.
Creating and monitoring process.
new
6> revolver ! 1.
click
1
7> revolver ! 3.
bang.
The shooter <0.50.0> died with reason {roulette,die,at,{18,55,57}}.3
 Restarting. 
 Creating and monitoring process.
8> revolver ! 4.
click
4
```

## Reference

* [Erlang website](http://www.erlang.org/)
* [Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages](http://www.amazon.com/Seven-Languages-Weeks-Programming-Programmers/dp/193435659X)
