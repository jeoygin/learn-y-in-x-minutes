# Learn Clojure in x minutes

Clojure is a dynamic programming language that targets the Java Virtual Machine (and the CLR, and JavaScript). It is designed to be a general-purpose language, combining the approachability and interactive development of a scripting language with an efficient and robust infrastructure for multithreaded programming. Clojure is a compiled language - it compiles directly to JVM bytecode, yet remains completely dynamic. Every feature supported by Clojure is supported at runtime. Clojure provides easy access to the Java frameworks, with optional type hints and type inference, to ensure that calls to Java can avoid reflection.

Clojure is a dialect of Lisp, and shares with Lisp the code-as-data philosophy and a powerful macro system. Clojure is predominantly a functional programming language, and features a rich set of immutable, persistent data structures. When mutable state is needed, Clojure offers a software transactional memory system and reactive Agent system that ensure clean, correct, multithreaded designs.

## Installation

Use [Leiningen](http://leiningen.org/) to manage Clojure project and java configuration.

## Glance

```sh
$ lein repl
REPL-y 0.3.5, nREPL 0.2.6
Clojure 1.6.0

user=> (println "Give me some Clojure!")
Give me some Clojure!
nil
user=> (- 1)
-1
user=> (+ 1 1)
2
user=> (* 10 10)
100
user=> (/ 1 3)
1/3
user=> (/ 2 4)
1/2
user=> (/ 2.0 4)
0.5
user=> (class (/ 1 3))
clojure.lang.Ratio
user=> (mod 5 4)
1
user=> (/ (/ 12 2) (/ 6 2))
2
user=> (+ 2 2 2 2)
8
user=> (- 8 1 2)
5
user=> (/ 8 2 2)
2
user=> (< 1 2 3)
true
user=> (< 1 3 2 4)
false
user=> (+ 3.0 5)
8.0
user=> (+ 3 5.0)
8.0
```

## String

```sh
user=> (println "master yoda\nluke skywalker\ndarth vader")
master yoda
luke skywalker
darth vader
nil
user=> (str 1)
"1"
user=> (str "yoda, " "luke, " "darth")
"yoda, luke, darth"
user=> (str "one: " 1 " two: " 2)
"one: 1 two: 2"
user=> \a
\a
user=> (str \f \o \r \c \e)
"force"
user=> (= "a" \a)
false
user=> (= (str \a) "a")
true
```

## Bool

```sh
user=> (= 1 2)
false
user=> (= 1 1.0)
false
user=> (class true)
java.lang.Boolean
user=> (class (= 1 1))
java.lang.Boolean
user=> (if true (println "True it is."))
True it is.
nil
user=> (if (> 1 2) (println "True it is."))
nil
user=> (if (< 1 2)
  #_=> (println "False it is not."))
False it is not.
nil
user=> (if false (println "true") (println "false"))
false
nil
user=> (if 0 (println "true"))
true
nil
user=> (if nil (println "true"))
nil
user=> (if "" (println "true"))
true
nil
```

## Collection

### List

```sh
user=> (1 2 3)

ClassCastException java.lang.Long cannot be cast to clojure.lang.IFn  user/eval841 (NO_SOURCE_FILE:1)
user=> (list 1 2 3)
(1 2 3)
user=> '(1 2 3)
(1 2 3)
user=> (first '(:r2d2 :c3po))
:r2d2
user=> (last '(:r2d2 :c3po))
:c3po
user=> (rest '(:r2d2 :c3po))
(:c3po)
user=> (cons :battle-droid '(:r2d2 :c3po))
(:battle-droid :r2d2 :c3po)
```

### Vector

```sh
user=> [:tom :mike :jean]
[:tom :mike :jean]
user=> (first [:tom :mike :jean])
:tom
user=> (nth [:tom :mike :jean] 2)
:jean
user=> (nth [:tom :mike :jean] 0)
:tom
user=> (last [:tom :mike :jean])
:jean
user=> (rest [:tom :mike :jean])
(:mike :jean)
user=> ([:tom :mike :jean] 2)
:jean
user=> (concat [:tiger] [:horse])
(:tiger :horse)
```

### Set

```sh
user=> #{:bird :cat :dog}
#{:cat :bird :dog}
user=> (def zoo #{:cat :bird :dog})
#'user/zoo
user=> zoo
#{:cat :bird :dog}
user=> (count zoo)
3
user=> (sort zoo)
(:bird :cat :dog)
user=> (sorted-set 2 3 1)
#{1 2 3}
user=> (clojure.set/union #{:mountain} #{:lake})
#{:lake :mountain}
user=> (clojure.set/difference #{1 2 3} #{2})
#{1 3}
user=> (#{:lake :mountain} :lake)
:lake
user=> (#{:lake :mountain} :ocean)
nil
```

### Map

```sh
user=> {:name :jeoygin :gender :male}
{:name :jeoygin, :gender :male}
user=> {:name :jeoygin :gender}

RuntimeException Map literal must contain an even number of forms  clojure.lang.Util.runtimeException (Util.java:221)
user=> {:name :jeoygin, :gender :male}
{:name :jeoygin, :gender :male}
user=> (def user {:name :jeoygin, :gender :male})
#'user/user
user=> user
{:name :jeoygin, :gender :male}
user=> (user :name)
:jeoygin
user=> (:name user)
:jeoygin
user=> (merge {:name :jeoygin} {:gender :male})
{:gender :male, :name :jeoygin}
user=> (merge-with + {:x 4, :y 2} {:z 2 :x 3})
{:z 2, :y 2, :x 7}
user=> (assoc {:one 1} :two 2)
{:two 2, :one 1}
user=> (sorted-map 1 :one, 3 :three, 2 :two)
{1 :one, 2 :two, 3 :three}
```

## Function

```sh
user=> (defn force-it [] (str "Use the force, " "Luke."))
#'user/force-it
user=> (force-it)
"Use the force, Luke."
user=> (defn force-it
  #_=> "The first function a young Jedi needs"
  #_=> []
  #_=> (str "use the force," "Luke"))
#'user/force-it
user=> (doc force-it)
-------------------------
user/force-it
([])
  The first function a young Jedi needs
nil
user=> (defn force-it [jedi] (str "Use the force, " jedi))
#'user/force-it
user=> (force-it "Jeoygin")
"Use the force, Jeoygin"
user=> (doc str)
-------------------------
clojure.core/str
([] [x] [x & ys])
  With no args, returns the empty string. With one arg x, returns
  x.toString().  (str nil) returns the empty string. With more than
  one arg, returns the concatenation of the str values of the args.
nil
```

## Binding

```sh
user=> (def line [[0 0] [10 20]])
#'user/line
user=> line
[[0 0] [10 20]]
user=> (defn line-end [ln] (last ln))
#'user/line-end
user=> (line-end line)
[10 20]
user=> (defn line-end [[_ second]] second)
#'user/line-end
user=> (line-end line)
[10 20]
user=> (def board [[:x :o :x] [:o :x :o] [:o :x :o]])
#'user/board
user=> (defn center [[_ [_ c _] _]] c)
#'user/center
user=> (center board)
:x
user=> (defn center [[_ [_ c]]] c)
#'user/center
user=> (defn center [board]
  #_=> (let [[_ [_ c]] board] c))
user=> (def person {:name "Jeoygin", :profession "Engineer"})
#'user/person
user=> (let [{name :name} person] (str "The person's name is " name))
"The person's name is Jeoygin"
```

## Anonymous Function

```sh
user=> (def people ["Jean", "Amy"])
#'user/people
user=> (count "Kim")
3
user=> (map count people)
(4 3)
user=> (defn twice-count [w] (* 2 (count w)))
user=> (twice-count "David")
10
user=> (map twice-count people)
(8 6)
user=> (map (fn [w] (* 2 (count w))) people)
(8 6)
user=> (map #(* 2 (count %)) people)
(8 6)
user=> (def v [3 1 2])
#'user/v
user=> (apply + v)
6
user=> (apply max v)
3
user=> (filter odd? v)
(3 1)
user=> (filter #(< % 3) v)
(1 2)
```

## Loop & Recur

```sh
user=> (defn size [v]
  #_=>     (if (empty? v)
  #_=>         0
  #_=>         (inc (size (rest v)))))
#'user/size
user=> (size [1 2 3])
3
user=> (loop [x 1] x)
1
user=> (defn size [v]
  #_=>     (loop [l v, c 0]
  #_=>     (if (empty? l)
  #_=>         c
  #_=>         (recur (rest l) (inc c)))))
#'user/size
```

## Sequence

```sh
user=> (every? number? [1 2 3 :four])
false
user=> (some nil? [1 2 nil])
true
user=> (not-every? odd? [1 3 5])
false
user=> (not-any? number? [:one :two :three])
true
user=> (def words ["luke" "chewie" "han" "lando"])
#'user/words
user=> (filter (fn [word] (> (count word) 4)) words)
("chewie" "lando")
user=> (map (fn [x] (* x x)) [1 1 2 3 5])
(1 1 4 9 25)
user=> (def colors ["red" "blue"])
#'user/colors
user=> (def toys ["block" "car"])
#'user/toys
user=> (for [x colors] (str "I like " x))
("I like red" "I like blue")
user=> (for [x colors, y toys] (str "I like " x " " y "s"))
("I like red blocks" "I like red cars" "I like blue blocks" "I like blue cars")
user=> (defn small-word? [w] (< (count w) 4))
#'user/small-word?
user=> (for [x colors, y toys, :when (small-word? y)]
  #_=>      (str "I like " x " " y "s"))
("I like red cars" "I like blue cars")
user=> (reduce + [1 2 3 4])
10
user=> (reduce * [1 2 3 4 5])
120
user=> (sort [3 1 2 4])
(1 2 3 4)
user=> (defn abs [x] (if (< x 0) (- x) x))
#'user/abs
user=> (sort-by abs [-1 -4 3 2])
(-1 2 3 -4)
```

## Lazy Evaluation

```sh
user=> (range 1 10)
(1 2 3 4 5 6 7 8 9)
user=> (range 1 10 3)
(1 4 7)
user=> (range 10)
(0 1 2 3 4 5 6 7 8 9)
user=> (take 3 (repeat "Use the Force, Luke"))
("Use the Force, Luke" "Use the Force, Luke" "Use the Force, Luke")
user=> (take 5 (cycle [:lather :rinse :repeat]))
(:lather :rinse :repeat :lather :rinse)
user=> (take 5 (drop 2 (cycle [:lather :rinse :repeat])))
(:repeat :lather :rinse :repeat :lather)
user=> (->> [:lather :rinse :repeat] (cycle) (drop 2) (take 5))
(:repeat :lather :rinse :repeat :lather)
user=> (take 5 (interpose :and (cycle [:lather :rinse :repeat])))
(:lather :and :rinse :and :repeat)
user=> (take 20 (interleave (cycle (range 2)) (cycle (range 3))))
(0 0 1 1 0 2 1 0 0 1 1 2 0 0 1 1 0 2 1 0)
user=> (take 5 (iterate inc 1))
(1 2 3 4 5)
user=> (take 5 (iterate dec 0))
(0 -1 -2 -3 -4)
user=> (defn fib-pair [[a b]]  [b (+ a b)])
#'user/fib-pair
user=> (fib-pair [3 5])
[5 8]
user=> (take 5
  #_=> (map
  #_=> first
  #_=> (iterate fib-pair [1 1])))
(1 1 2 3 5)
user=> (nth (map first (iterate fib-pair [1 1])) 50)
20365011074
user=> (defn factorial [n] (apply * (take n (iterate inc 1))))
#'user/factorial
user=> (factorial 5)
120
```

## Defrecord & Protocols

```sh
user=> (defprotocol Compass
  #_=>     (direction [c])
  #_=>     (left [c])
  #_=>     (right [c]))
Compass
user=> (def directions [:north :east :south :west])
#'user/directions
user=> (defn turn [base amount] (rem (+ base amount) (count directions)))
#'user/turn
user=> (turn 1 1)
2
user=> (turn 3 1)
0
user=> (turn 2 3)
1
user=> (defrecord SimpleCompass [bearing]
  #_=>     Compass
  #_=>     (direction [_] (directions bearing))
  #_=>     (left [_] (SimpleCompass. (turn bearing 3)))
  #_=>     (right [_] (SimpleCompass. (turn bearing 1)))
  #_=>     Object
  #_=>     (toString [this] (str "[" (direction this) "]")))
user.SimpleCompass
user=> (def c (SimpleCompass. 0))
#'user/c
user=> (left c)
#user.SimpleCompass{:bearing 3}
user=> c
#user.SimpleCompass{:bearing 0}
user=> (:bearing c)
0
```

## Macros

```sh
user=> (defn unless [test body] (if (not test) body))
#'user/unless
user=> (unless true (println "Danger, danger Will Robinson"))
Danger, danger Will Robinson
nil
user=> (macroexpand '#(count %))
(fn* [p1__1019#] (count p1__1019#))
user=> (defmacro unless [test body]
  #_=>     (list 'if (list 'not test) body))
#'user/unless
user=> (macroexpand '(unless condition body))
(if (not condition) body)
user=> (unless true (println "No more danger, Will."))
nil
user=> (unless false (println "Now, THIS is The FORCE."))
Now, THIS is The FORCE.
nil
```

## References and Transactional Memory

```sh
user=> (ref "Attack of the Clones")
#<Ref@61b5b044: "Attack of the Clones">
user=> (def movie (ref "Star Wars"))
#'user/movie
user=> movie
#<Ref@30f8f67d: "Star Wars">
user=> (deref movie)
"Star Wars"
user=> @movie
"Star Wars"
user=> (alter movie str ": The Empire Strikes Back")

IllegalStateException No transaction running  clojure.lang.LockingTransaction.getEx (LockingTransaction.java:208)
user=> (dosync (alter movie str ": The Empire Strikes Back"))
"Star Wars: The Empire Strikes Back"
user=> (dosync (ref-set movie "Star Wars: The Revenge of the Sith"))
"Star Wars: The Revenge of the Sith"
user=> @movie
"Star Wars: The Revenge of the Sith"
```

## Atoms

```sh
user=> (atom "Split at your own risk.")
#<Atom@21d3b6e9: "Split at your own risk.">
user=> (def danger (atom "Split at your own risk."))
#'user/danger
user=> danger
#<Atom@677b3bb5: "Split at your own risk.">
user=> @danger
"Split at your own risk."
user=> (reset! danger "Split with impunity")
"Split with impunity"
user=> danger
#<Atom@677b3bb5: "Split with impunity">
user=> @danger
"Split with impunity"
user=> (def top-sellers (atom []))
#'user/top-sellers
user=> (swap! top-sellers conj {:title "Seven Languages", :author "Tate"})
[{:title "Seven Languages", :author "Tate"}]
user=> (swap! top-sellers conj {:title "Programming Clojure" :author "Halloway"})
[{:title "Seven Languages", :author "Tate"} {:title "Programming Clojure", :author "Halloway"}]
user=> (defn create [] (atom {}))
#'user/create
user=> (defn _get [cache key] (@cache key))
#'user/_get
user=> (defn _put
  #_=>     ([cache value-map] (swap! cache merge value-map))
  #_=>     ([cache key value] (swap! cache assoc key value)))
#'user/_put
user=> (def ac (create))
#'user/ac
user=> (_put ac :quote "I'm your father, Luke.")
{:quote "I'm your father, Luke."}
user=> (println (str "Cached item: " (_get ac :quote)))
Cached item: I'm your father, Luke.
nil
```

## Agents

```sh
user=> (defn twice [x] (* 2 x))
#'user/twice
user=> (def tribbles (agent 1))
#'user/tribbles
user=> (send tribbles twice)
#<Agent@47cd1c75: 2>
user=> @tribbles
2
user=> (defn slow-twice [x]
  #_=>     (do
  #_=>         (Thread/sleep 5000)
  #_=>         (* 2 x)))
#'user/slow-twice
user=> @tribbles
2
user=> (send tribbles slow-twice)
#<Agent@47cd1c75: 2>
user=> @tribbles
2
user=> ; do this five seconds later
user=> @tribbles
4
```

## Futures

```sh
user=> (def finer-things (future (Thread/sleep 5000) "take time"))
#'user/finer-things
user=> ; you may have to wait for the result
user=> @finer-things
"take time"
```

## Reference

* [七周七语言:理解多种编程范型](http://www.amazon.cn/%E4%B8%83%E5%91%A8%E4%B8%83%E8%AF%AD%E8%A8%80-%E7%90%86%E8%A7%A3%E5%A4%9A%E7%A7%8D%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B-%E6%B3%B0%E7%89%B9/dp/B008041DUY)