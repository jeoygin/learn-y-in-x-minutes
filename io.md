# Learn Io in x minutes

Io is a prototype-based programming language inspired by Smalltalk (all values are objects, all messages are dynamic), Self (prototype-based), NewtonScript (differential inheritance), Act1 (actors and futures for concurrency), LISP (code is a runtime inspectable/modifiable tree) and Lua (small, embeddable).

## Installation

It's easy for you to install Io with homebrew package manager on OSX:

```sh
$ brew install io
```

Please refer to [http://iolanguage.org/](http://iolanguage.org/) for more details.

## Glance

```sh
$ io
Io 20110905
Io> "Hi ho, Io" print
Hi ho, Io==> Hi ho, Io
Io> Vehicle := Object clone
==>  Vehicle_0x7ff48423ce30:
  type             = "Vehicle"
Io> Vehicle description := "Something to take you places"
==> Something to take you places
Io> Vehicle description = "Something to take you far away"
==> Something to take you far away
Io> Vehicle nonexistingSlot = "This won't work."

  Exception: Slot nonexistingSlot not found. Must define slot using := operator before updating.
  ---------
  message 'updateSlot' in 'Command Line' on line 1
Io> Vehicle description
==> Something to take you far away
Io> Vehicle slotNames
==> list(type, description)
Io> Vehicle type
==> Vehicle
Io> Object type
==> Object
```

## Object

```sh
Io> Car := Vehicle clone
==>  Car_0x7ff484252760:
  type             = "Car"

Io> Car slotNames
==> list(type)
Io> Car type
==> Car
Io> Car description
==> Something to take you far away
Io> porsche := Car clone
==>  Car_0x7ff484196e90:

Io> porsche slotNames
==> list()
Io> porsche type
==> Car
Io> Porsche := Car clone
==>  Porsche_0x7ff48244ac70:
  type             = "Porsche"

Io> Porsche slotNames
==> list(type)
Io> Porsche type
==> Porsche
```

Object is a container of slots. Slot can be attained by sending name of it. If the slot is not existed, parent object's slot whose name is the same will be called. No class, no interface, no module. Everything is object.

## Method

```sh
Io> method("so, you've come for an argument." println)
==> method(
    "so, you've come for an argument." println
)
Io> method() type
==> Block
Io> method type
==> Block
Io> Car drive := method("Vroom" println)
==> method(
    "Vroom" println
)
Io> porsche drive
Vroom
==> Vroom
Io> porsche getSlot("drive")
==> method(
    "Vroom" println
)
Io> porsche getSlot("type")
==> Car
Io> porsche proto
==>  Car_0x7ff484252760:
  drive            = method(...)
  type             = "Car"

Io> Car proto
==>  Vehicle_0x7ff48423ce30:
  description      = "Something to take you far away"
  type             = "Vehicle"
Io> Lobby
==>  Object_0x7ff48241d530:
  Car              = Car_0x7ff484252760
  Lobby            = Object_0x7ff48241d530
  Protos           = Object_0x7ff48241c820
  Vehicle          = Vehicle_0x7ff48423ce30
  exit             = method(...)
  forward          = method(...)
  porsche          = Car_0x7ff484196e90
```

## List

```sh
Io> toDos := list("find my car", "find Continuum Transfunctioner")
==> list(find my car, find Continuum Transfunctioner)
Io> toDos size
==> 2
Io> toDos append("Find a present")
==> list(find my car, find Continuum Transfunctioner, Find a present)
Io> list(1, 2, 3, 4)
==> list(1, 2, 3, 4)
Io> list(1, 2, 3, 4) average
==> 2.5
Io> list(1, 2, 3, 4) sum
==> 10
Io> list(1, 2, 3, 4) at(1)
==> 2
Io> list(1, 2, 3) append(4)
==> list(1, 2, 3, 4)
Io> list(1, 2, 3) pop
==> 3
Io> list(1, 2, 3) prepend(0)
==> list(0, 1, 2, 3)
Io> list() isEmpty
==> true
```

## Map

```sh
Io> elvis := Map clone
==>  Map_0x7ff484243050:

Io> elvis atPut("home", "Graceland")
==>  Map_0x7ff484243050:

Io> elvis at("home")
==> Graceland
Io> elvis atPut("style", "rock and roll")
==>  Map_0x7ff484243050:

Io> elvis asObject
==>  Object_0x7ff48413f560:
  home             = "Graceland"
  style            = "rock and roll"

Io> elvis asList
==> list(list(home, Graceland), list(style, rock and roll))
Io> elvis keys
==> list(home, style)
Io> elvis size
==> 2
```

## true, false, nil and singleton

```sh
Io> 4 < 5
==> true
Io> 4 <= 3
==> false
Io> true and false
==> false
Io> true and true
==> true
Io> true or true
==> true
Io> true or false
==> true
Io> 4 < 5 and 6 > 7
==> false
Io> true and 6
==> true
Io> true and 0
==> true
Io> true proto
==>  Object_0x7ff4824052e0:
                   = Object_()
  !=               = Object_!=()
  -                = Object_-()
...
Io> true clone
==> true
Io> false clone
==> false
Io> nil clone
==> nil
Io> Singleton := Object clone
==>  Singleton_0x7ff484281420:
  type             = "Singleton"

Io> Singleton clone := Singleton
==>  Singleton_0x7ff484281420:
  clone            = Singleton_0x7ff484281420
  type             = "Singleton"

Io> Singleton clone
==>  Singleton_0x7ff484281420:
  clone            = Singleton_0x7ff484281420
  type             = "Singleton"

Io> mike := Singleton clone
==>  Singleton_0x7ff484281420:
  clone            = Singleton_0x7ff484281420
  type             = "Singleton"

```

## Conditions & Loops

```sh
Io> loop("getting dizzy..." println) getting dizzy...
getting dizzy...
...
getting dizzy.^C
IoVM:
        Received signal. Setting interrupt flag.
Io> i := 1
==> 1
Io> while(i <= 11, i println; i = i + 1); "This one goes up to 11" println
1
2
3
4
5
6
7
8
9
10
11
This one goes up to 11
==> This one goes up to 11
Io> for(i, 1, 11, i println); "This one goes up to 11" println
1
2
3
4
5
6
7
8
9
10
11
This one goes up to 11
==> This one goes up to 11
Io> for(i, 1, 11, 2, i println); "This one goes up to 11" println
1
3
5
7
9
11
This one goes up to 11
==> This one goes up to 11
Io> for(i, 1, 2, 1, i println, "extra argument")
1
2
==> 2
Io> for(i, 1, 2, i println, "extra argument")
2
==> extra argument
Io> if(true, "It is true.", "It is false.")
==> It is true.
Io> if(false) then("It is true") else("It is false")
==> nil
Io> if(false) then("It is true." println) else("It is false." println)
It is false.
==> nil
```

## Operators

```sh
Io> OperatorTable
==> OperatorTable_0x7fb160d37f20:
Operators
  0   ? @ @@
  1   **
  2   % * /
  3   + -
  4   << >>
  5   < <= > >=
  6   != ==
  7   &
  8   ^
  9   |
  10  && and
  11  or ||
  12  ..
  13  %= &= *= += -= /= <<= >>= ^= |=
  14  return

Assign Operators
  ::= newSlot
  :=  setSlot
  =   updateSlot

To add a new operator: OperatorTable addOperator("+", 4) and implement the + message.
To add a new assign operator: OperatorTable addAssignOperator("=", "updateSlot") and implement the updateSlot message.

Io> OperatorTable addOperator("xor", 11)
==> OperatorTable_0x7fb160d37f20:
Operators
  ...
  11  or xor ||
  12  ..
  ...
Io> true xor := method(bool, if(bool, false, true))
==> method(bool,
    if(bool, false, true)
)
Io> false xor := method(bool, if(bool, true, false))
==> method(bool,
    if(bool, true, false)
)
Io> true xor true
==> false
Io> true xor false
==> true
Io> false xor true
==> true
Io> false xor false
==> false
```

## Messages

```sh
Io> postOffice := Object clone
==>  Object_0x7fb160f41150:

Io> postOffice packageSender := method(call sender)
==> method(
    call sender
)
Io> mailer := Object clone
==>  Object_0x7fb160fab5d0:

Io> mailer deliver := method(postOffice packageSender)
==> method(
    postOffice packageSender
)
Io> mailer deliver
==>  Object_0x7fb160fab5d0:
  deliver          = method(...)

Io> postOffice messageTarget := method(call target)
==> method(
    call target
)
Io> postOffice messageTarget
==>  Object_0x7fb160f41150:
  messageTarget    = method(...)
  packageSender    = method(...)

Io> postOffice messageArgs := method(call message arguments)
==> method(
    call message arguments
)
Io> postOffice messageName := method(call message name)
==> method(
    call message name
)
Io> postOffice messageArgs("one", 2, :three)
==> list("one", 2, : three)
Io> postOffice messageName
==> messageName
Io> unless := method(
...     (call sender doMessage(call message argAt(0))) ifFalse(
...     call sender doMessage(call message argAt(1))) ifTrue(
...     call sender doMessage(call message argAt(2)))
... )
==> method(
    (call sender doMessage(call message argAt(0))) ifFalse(call sender doMessage(call message argAt(1))) ifTrue(call sender doMessage(call message argAt(2)))
)
Io> unless(1 == 2, write("One is not two\n"), write("one is two\n"))
One is not two
==> false
```

## Relection

`animals.io`

```io
Object ancestors := method(
        prototype := self proto
        if(prototype != Object,
        writeln("Slots of ", prototype type, "\n---------------")
        prototype slotNames foreach(slotName, writeln(slotName))
        writeln
        prototype ancestors))
Animal := Object clone
Animal speak := method("ambiguous animal noise" println)
Duck := Animal clone
Duck speak := method("quack" println)
Duck walk := method("waddle" println)
disco := Duck clone
disco ancestors
```

```sh
$ io animals.io
Slots of Duck
---------------
type
walk
speak

Slots of Animal
---------------
type
speak
```

## Domain-Specific Languages

`phonebook.txt`

```
{
    "Bob Smith": "5195551212",
    "Mary Walsh": "4162223434"
}
```

`phonebook.io`

```io
OperatorTable addAssignOperator(":", "atPutNumber")
curlyBrackets := method(
  r := Map clone
  call message arguments foreach(arg,
       r doMessage(arg)
       )
  r
)
Map atPutNumber := method(
  self atPut(
       call evalArgAt(0) asMutable removePrefix("\"") removeSuffix("\""),
       call evalArgAt(1))
)
s := File with("phonebook.txt") openForReading contents
phoneNumbers := doString(s)
phoneNumbers keys println
phoneNumbers values println
```

```sh
$ io phonebook.io
list(Bob Smith, Mary Walsh)
list(5195551212, 4162223434)
```

## method_missing

`builder.io`

```io
Builder := Object clone
Builder forward := method(
  writeln("<", call message name, ">")
  call message arguments foreach(
  arg,
  content := self doMessage(arg);
  if(content type == "Sequence", writeln(content)))
  writeln("</", call message name, ">"))
Builder  ul(
  li("Io"),
  li("Lua"),
  li("JavaScript"))
```

```sh
$ io builder.io
<ul>
<li>
Io
</li>
<li>
Lua
</li>
<li>
JavaScript
</li>
</ul>
```

## Concurrency

### Coroutines

`coroutine.io`

```io
vizzini := Object clone
vizzini talk := method(
            "Fezzik, are there rocks ahead?" println
            yield
            "No more rhymes now, I mean it." println
             yield)

fezzik := Object clone

fezzik rhyme := method(
                        yield
            "If there are, we'll all be dead." println
            yield
            "Anybody want a peanut?" println)

vizzini @@talk; fezzik @@rhyme

Coroutine currentCoroutine pause
```

```sh
$ io coroutine.io
Fezzik, are there rocks ahead?
If there are, we'll all be dead.
No more rhymes now, I mean it.
Anybody want a peanut?
Scheduler: nothing left to resume so we are exiting
```

### Actors

```sh
Io> slower := Object clone
==>  Object_0x7fa395805280:

Io> faster := Object clone
==>  Object_0x7fa393f428f0:

Io> slower start := method(wait(2); writeln("slowly"))
==> method(
    wait(2); writeln("slowly")
)
Io> faster start := method(wait(1); writeln("quickly"))
==> method(
    wait(1); writeln("quickly")
)
Io> slower start; faster start
slowly
quickly
==> nil
Io> slower @@start; faster @@start; wait(3)
quickly
slowly
==> nil
```

### Futures

```io
futureResult := URL with("http://jeoygin.org/") @fetch
writeln("Do something immediately while fetch goes on in background...")
writeln("This will block until the result is available.")
writeln("fetched ", futureResult size, " bytes")
// this will block until the computation is complete
```

## Reference

* [Io Programming Guide](http://iolanguage.org/scm/io/docs/IoGuide.html)
* [Io Tutorial](http://iolanguage.org/scm/io/docs/IoTutorial.html)
* [七周七语言:理解多种编程范型](http://www.amazon.cn/%E4%B8%83%E5%91%A8%E4%B8%83%E8%AF%AD%E8%A8%80-%E7%90%86%E8%A7%A3%E5%A4%9A%E7%A7%8D%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B-%E6%B3%B0%E7%89%B9/dp/B008041DUY)
