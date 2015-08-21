# Learn Scala in x minutes

Scala is an acronym for “Scalable Language”. This means that Scala grows with you. You can play with it by typing one-line expressions and observing the results. But you can also rely on it for large mission critical systems, as many companies, including Twitter, LinkedIn, or Intel do.

**Object-Oriented Meets Functional**

Have the best of both worlds. Construct elegant class hierarchies for maximum code reuse and extensibility, implement their behavior using higher-order functions. Or anything in-between.

[TOC]

## Installation

You can download Scala from [Official Download](http://www.scala-lang.org/download/).

If you are a user of OSX, you als can install Scala using brew:

    $ brew install scala

## Fundamental Scala

### Types

```scala
$ scala
scala> println("Hello world")
Hello world

scala> 1 + 1
res1: Int = 2

scala> (1).+(1)
res2: Int = 2

scala> 1 + 1.0
res3: Double = 2.0

scala> 1 + 2 * 3
res4: Int = 7

scala> 1.+(1)
res5: Int = 2

scala> 1.+(2.*(3))
res6: Int = 7

scala> 5.+(4.*(3))
res7: Int = 17

scala> "hello".size
res8: Int = 5

scala> "hello" + 4
res9: String = hello4

scala> 4 + "hello"
res10: String = 4hello

scala> 4 + ".0"
res11: String = 4.0

scala> 4 * "1"
<console>:8: error: overloaded method value * with alternatives:
  (x: Double)Double <and>
  (x: Float)Float <and>
  (x: Long)Long <and>
  (x: Int)Int <and>
  (x: Char)Int <and>
  (x: Short)Int <and>
  (x: Byte)Int
 cannot be applied to (String)
              4 * "1"
                ^
```

### Expressions and Conditions

```scala
scala> 1 < 2
res13: Boolean = true

scala> 1 <= 2
res14: Boolean = true

scala> 2 <= 1
res15: Boolean = false

scala> 1 != 2
res16: Boolean = true

scala> a = 1
<console>:10: error: not found: value a
val $ires0 = a
             ^
<console>:7: error: not found: value a
       a = 1
       ^

scala> val a = 1
a: Int = 1

scala> a = 2
<console>:8: error: reassignment to val
       a = 2
         ^

scala> val b = 2
b: Int = 2

scala> if (b < a) {
     |   println("true")
     | } else {
     |   println("false")
     | }
false

scala> Nil
res18: scala.collection.immutable.Nil.type = List()

scala> if (0) println("true")
<console>:8: error: type mismatch;
 found   : Int(0)
 required: Boolean
              if (0) println("true")
                  ^

scala> if (Nil) println("true")
<console>:8: error: type mismatch;
 found   : scala.collection.immutable.Nil.type
 required: Boolean
              if (Nil) println("true")
                  ^
```

### Loops

`while.scala`

```scala
def whileLoop {
    var i = 1
    while (i <= 3) {
          println(i)
          i += 1
    }
}

whileLoop
```

```bash
$ scala while.scala
1
2
3
```

`for_loop.scala`

```scala
def forLoop {
    println( "for loop using Java-style iteration" )
    for (i <- 0 until args.length) {
        println(args(i))
    }
}

forLoop
```

```bash
$ scala for_loop.scala hello world
for loop using Java-style iteration
hello
world
```

`ruby_for_loop.scala`

```scala
def rubyStyleForLoop {
    println( "for loop using Ruby-style iteration" )
    args.foreach { arg =>
        println(arg)
    }
}

rubyStyleForLoop
```

```bash
$ scala ruby_for_loop.scala hello world
for loop using Ruby-style iteration
hello
world
```

### Ranges and Tuples

```scala
scala> val range = 0 until 10
range: scala.collection.immutable.Range = Range(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)

scala> range.start
res0: Int = 0

scala> range.end
res1: Int = 10

scala> range.step
res2: Int = 1

scala> (0 to 10) by 5
res3: scala.collection.immutable.Range = Range(0, 5, 10)

scala> (0 to 10) by 6
res4: scala.collection.immutable.Range = Range(0, 6)

scala> (0 until 10 by 5)
res5: scala.collection.immutable.Range = Range(0, 5)

scala> val range = (10 until 0) by -1
range: scala.collection.immutable.Range = Range(10, 9, 8, 7, 6, 5, 4, 3, 2, 1)

scala> val range = (10 until 0)
range: scala.collection.immutable.Range = Range()

scala> val range = (0 to 10)
range: scala.collection.immutable.Range.Inclusive = Range(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

scala> val range = 'a' to 'e'
range: scala.collection.immutable.NumericRange.Inclusive[Char] = NumericRange(a, b, c, d, e)

scala> person._1
res6: String = Elvis

scala> person._2
res7: String = Presley

scala> person._3
<console>:9: error: value _3 is not a member of (String, String)
              person._3
                     ^

scala> val (x, y) = (1, 2)
x: Int = 1
y: Int = 2

scala> val (a, b) = (1, 2, 3)
<console>:7: error: constructor cannot be instantiated to expected type;
 found   : (T1, T2)
 required: (Int, Int, Int)
       val (a, b) = (1, 2, 3)
           ^
```

## Classes in Scala

```scala
scala> class Person(firstName: String, lastName: String)
defined class Person

scala> val jeoygin = new Person("Jeoygin", "Wang")
jeoygin: Person = Person@782830e
```

`compass.scala`

```
class Compass {
    val directions = List("north", "east", "south", "west")
    var bearing = 0

    print("Initial bearing: ")
    println(direction)

    def direction() = directions(bearing)

    def inform(turnDirection: String) {
        println("Turning " + turnDirection + ". Now bearing " + direction)
    }

    def turnRight() {
        bearing = (bearing + 1) % directions.size
        inform("right" )
    }

    def turnLeft() {
        bearing = (bearing + (directions.size - 1)) % directions.size
        inform("left" )
    }
}

val myCompass = new Compass

myCompass.turnRight
myCompass.turnRight

myCompass.turnLeft
myCompass.turnLeft
myCompass.turnLeft
```

```bash
$ scala compass.scala
Initial bearing: north
Turning right. Now bearing east
Turning right. Now bearing south
Turning left. Now bearing east
Turning left. Now bearing north
Turning left. Now bearing west
```

### Auxiliary Constructors

`constructors.scala`

```scala
class Person(first_name: String) {
    println("Outer constructor")
    def this(first_name: String, last_name: String) {
        this(first_name)
        println("Inner constructor")
    }
    def talk() = println("Hi")
}

val bob = new Person("Bob")
val bobTate = new Person("Bob", "Tate")
```

```bash
$ scala constructors.scala
Outer constructor
Outer constructor
Inner constructor
```

### Companion Objects and Class Methods

`ring.scala`

```scala
object TrueRing {
    def rule = println("To rule them all")
}

TrueRing.rule
```

### Inheritance

`employee.scala`

```scala
class Person(val name: String) {
    def talk(message: String) = println(name + " says " + message) 
    def id(): String = name
}
class Employee(override val name: String,
                        val number: Int) extends Person(name) {
    override def talk(message: String) {
        println(name + " with number " + number + " says " + message)
    }
    override def id():String = number.toString 
}

val employee = new Employee("Yoda", 4) 
employee.talk("Extend or extend not. There is no try.")
```

### Traits

`nice.scala`

```scala
class Person(val name:String)

trait Nice {
    def greet() = println("Hello world!")
}

class Character(override val name:String) extends Person(name) with Nice

val flanders = new Character("Ned")
flanders.greet
```

```bash
$ scala nice.scala
Hello world!
```

## Functions

```scala
scala> def double(x:Int):Int = x * 2
double: (x: Int)Int

scala> double(4)
res2: Int = 8

scala> def double(x:Int):Int = {
     |   x * 2
     | }
double: (x: Int)Int

scala> double(6)
res3: Int = 12
```

## var versus val

```scala
scala> var mutable = "I am mutable"
mutable: String = I am mutable

scala> mutable = "Change me..."
mutable: String = Change me...

scala> val immutable = "I am not mutable"
immutable: String = I am not mutable

scala> immutable = "Can't touch this"
<console>:8: error: reassignment to val
       immutable = "Can't touch this"
                 ^
```

## Collections

### Lists

```scala
scala> List(1, 2, 3)
res0: List[Int] = List(1, 2, 3)

scala> List("one", "two", "three")
res1: List[String] = List(one, two, three)

scala> List("one", "two", 3)
res2: List[Any] = List(one, two, 3)

scala> List("one", "two", 3)(2)
res3: Any = 3

scala> List("one", "two", 3)(4)
java.lang.IndexOutOfBoundsException: 4
  at scala.collection.LinearSeqOptimized$class.apply(LinearSeqOptimized.scala:51)
  at scala.collection.immutable.List.apply(List.scala:83)
  ... 33 elided

scala> List("one", "two", 3)(-1)
java.lang.IndexOutOfBoundsException: -1
  at scala.collection.LinearSeqOptimized$class.apply(LinearSeqOptimized.scala:51)
  at scala.collection.immutable.List.apply(List.scala:83)
  ... 33 elided

scala> Nil
res6: scala.collection.immutable.Nil.type = List()
```

### Sets

```scala
scala> val colors = Set("red", "green", "blue")
colors: scala.collection.immutable.Set[String] = Set(red, green, blue)

scala> colors + "yellow"
res7: scala.collection.immutable.Set[String] = Set(red, green, blue, yellow)

scala> colors - "green"
res8: scala.collection.immutable.Set[String] = Set(red, blue)

scala> colors + Set("black", "white")
<console>:9: error: type mismatch;
 found   : scala.collection.immutable.Set[String]
 required: String
              colors + Set("black", "white")
                          ^

scala> colors ++ Set("black", "white")
res10: scala.collection.immutable.Set[String] = Set(blue, green, black, white, red)

scala> colors -- Set("red", "green")
res11: scala.collection.immutable.Set[String] = Set(blue)

scala> Set(1, 2, 3) == Set(3, 2, 1)
res14: Boolean = true

scala> List(1, 2, 3) == List(3, 2, 1)
res15: Boolean = false
```

### Maps

```scala
scala> val ordinals = Map(0 -> "zero", 1 -> "one", 2 -> "two")
ordinals: scala.collection.immutable.Map[Int,String] = Map(0 -> zero, 1 -> one, 2 -> two)

scala> ordinals(2)
res16: String = two

scala> import scala.collection.mutable.HashMap
import scala.collection.mutable.HashMap

scala> val map = new HashMap[Int, String]
map: scala.collection.mutable.HashMap[Int,String] = Map()

scala> map += 4 -> "four"
res17: map.type = Map(4 -> four)

scala> map += 8 -> "eight"
res18: map.type = Map(8 -> eight, 4 -> four)

scala> map
res19: scala.collection.mutable.HashMap[Int,String] = Map(8 -> eight, 4 -> four)

scala> map += "zero" -> 0
<console>:10: error: type mismatch;
 found   : (String, Int)
 required: (Int, String)
              map += "zero" -> 0
                            ^
```

## Collections and Functions

### foreach

```scala
scala> val list = List("tiger", "cat", "dog")
list: List[String] = List(tiger, cat, dog)

scala> list.foreach(animal => println(animal))
tiger
cat
dog

scala> val animals = Set("tiger", "cat", "dog")
animals: scala.collection.immutable.Set[String] = Set(tiger, cat, dog)

scala> animals.foreach(animal => println(animal))
tiger
cat
dog

scala> val animals = Map("tiger" -> "animal", "cat" -> "animal", "dog" -> "animal")
animals: scala.collection.immutable.Map[String,String] = Map(tiger -> animal, cat -> animal, dog -> animal)

scala> animals.foreach(animal => println(animal))
(tiger,animal)
(cat,animal)
(dog,animal)

scala> animals.foreach(animal => println(animal._1))
tiger
cat
dog

scala> animals.foreach(animal => println(animal._2))
animal
animal
animal
```

### More List Methods

```scala
scala> list
res26: List[String] = List(tiger, cat, dog)

scala> list.isEmpty
res27: Boolean = false

scala> Nil.isEmpty
res28: Boolean = true

scala> list.length
res29: Int = 3

scala> list.size
res30: Int = 3

scala> list.head
res31: String = tiger

scala> list.tail
res32: List[String] = List(cat, dog)

scala> list.last
res33: String = dog

scala> list.init
res34: List[String] = List(tiger, cat)

scala> list.reverse
res35: List[String] = List(dog, cat, tiger)

scala> list.drop(1)
res36: List[String] = List(cat, dog)

scala> list
res37: List[String] = List(tiger, cat, dog)

scala> list.drop(2)
res38: List[String] = List(dog)
```

### count, map, filter, and Others

```scala
scala> val words = List("train", "plane", "car", "bike")
words: List[String] = List(train, plane, car, bike)

scala> words.count(word => word.size > 3)
res39: Int = 3

scala> words.filter(word => word.size > 3)
res40: List[String] = List(train, plane, bike)

scala> words.map(word => word.size)
res41: List[Int] = List(5, 5, 3, 4)

scala> words.forall(word => word.size > 2)
res42: Boolean = true

scala> words.exists(word => word.size > 4)
res43: Boolean = true

scala> words.exists(word => word.size > 5)
res44: Boolean = false

scala> words.sortWith((s, t) => s.charAt(0).toLower < t.charAt(0).toLower)
res49: List[String] = List(bike, car, plane, train)

scala> words.sortWith((s, t) => s.size < t.size)
res50: List[String] = List(car, bike, train, plane)
```

### foldLeft

```scala
scala> val sum = (0 /: list) {(sum, i) => sum + i}
sum: Int = 6

scala> list.foldLeft(0)((sum, value) => sum + value)
res51: Int = 6
```

## XML

```scala
scala> val movies =
     | <movies>
     |     <movie genre="action">Pirates of the Caribbean</movie>
     |     <movie genre="fairytale">Edward Scissorhands</movie>
     | </movies>
movies: scala.xml.Elem =
<movies>
    <movie genre="action">Pirates of the Caribbean</movie>
    <movie genre="fairytale">Edward Scissorhands</movie>
</movies>

scala> movies.text
res0: String =
"
    Pirates of the Caribbean
    Edward Scissorhands
"

scala> val movieNodes = movies \ "movie"
movieNodes: scala.xml.NodeSeq = NodeSeq(<movie genre="action">Pirates of the Caribbean</movie>, <movie genre="fairytale">Edward Scissorhands</movie>)

scala> movieNodes(0)
res1: scala.xml.Node = <movie genre="action">Pirates of the Caribbean</movie>

scala> movieNodes(0) \ "@genre"
res2: scala.xml.NodeSeq = action
```

## Pattern Matching

`chores.scala`

```scala
def doChore(chore: String): String = chore match { 
    case "clean dishes" => "scrub, dry"
    case "cook dinner" => "chop, sizzle"
    case _ => "whine, complain"
}
println(doChore("clean dishes")) 
println(doChore("mow lawn"))
```

```bash
$ scala chores.scala scrub, dry
whine, complain
```

### Guards

`factorial.scala`

```scala
def factorial(n: Int): Int = n match { 
    case 0 => 1
    case x if x > 0 => factorial(n - 1) * n 
}

println(factorial(3))
println(factorial(0))
```

### Regular Expressions

```scala
scala> val reg = """^(F|f)\w*""".r
reg: scala.util.matching.Regex = ^(F|f)\w*

scala> println(reg.findFirstIn("Fantastic"))
Some(Fantastic)

scala> println(reg.findFirstIn("not Fantastic"))
None

scala> val reg = "the".r
reg: scala.util.matching.Regex = the

scala> reg.findAllIn("the way the scissors trim the hair and the shrubs")
res5: scala.util.matching.Regex.MatchIterator = non-empty iterator
```

### XML with Matching

`movies.scala`

```scala
val movies = <movies>
    <movie>The Incredibles</movie>
    <movie>WALL E</movie>
    <short>Jack Jack Attack</short>
    <short>Geri's Game</short>
</movies>

(movies \ "_").foreach { movie => 
    movie match {
        case <movie>{movieName}</movie> => println(movieName)
        case <short>{shortName}</short> => println(shortName + " (short)") 
    }
}
```

## Concurrency

`kids.scala`

```scala
import scala.actors._
import scala.actors.Actor._

case object Poke
case object Feed

class Kid() extends Actor {
    def act() {
        loop {
            react {
                case Poke => {
                    println("Ow..." )
                    println("Quit it...")
                }
                case Feed => {
                    println("Gurgle..." )
                    println("Burp..." )
                }
            }
        }
    }
}
val bart = new Kid().start
val lisa = new Kid().start
println("Ready to poke and feed...")
bart ! Poke
lisa ! Poke
bart ! Feed
lisa ! Feed
```

```bash
$ scala kids.scala
Ready to poke and feed...
Ow...
Quit it...
Ow...
Quit it...
Gurgle...
Burp...
Gurgle...
Burp...
$ scala kids.scala
Ready to poke and feed...
Ow...
Quit it...
Gurgle...
Burp...
Ow...
Quit it...
Gurgle...
Burp...
```

### Concurrency in Action

`sizer.scala`

```scala
import scala.io._
import scala.actors._
import Actor._

object PageLoader {
 def getPageSize(url : String) = Source.fromURL(url).mkString.length
}

val urls = List("http://www.amazon.com/", 
               "http://www.twitter.com/",
               "http://www.google.com/",
               "http://www.cnn.com/" )

def timeMethod(method: () => Unit) = {
 val start = System.nanoTime
 method()
 val end = System.nanoTime
 println("Method took " + (end - start)/1000000000.0 + " seconds.")
}

def getPageSizeSequentially() = {
 for(url <- urls) {
   println("Size for " + url + ": " + PageLoader.getPageSize(url))
 }
}

def getPageSizeConcurrently() = {
 val caller = self

 for(url <- urls) {
   actor { caller ! (url, PageLoader.getPageSize(url)) }
 }

 for(i <- 1 to urls.size) {
   receive {
     case (url, size) =>
       println("Size for " + url + ": " + size)            
   }
 }
}

println("Sequential run:")
timeMethod { getPageSizeSequentially }

println("Concurrent run")
timeMethod { getPageSizeConcurrently }
```

## Reference

* [Scala Website](http://www.scala-lang.org/)
* [Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages](http://www.amazon.com/Seven-Languages-Weeks-Programming-Programmers/dp/193435659X)
