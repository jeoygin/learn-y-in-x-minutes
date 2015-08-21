# Learn Ruby in x minutes

Ruby is a dynamic, open source programming language with a focus on simplicity and productivity. It has an elegant syntax that is natural to read and easy to write.

## Installation

Ruby can be installed on many platforms (Linux, Mac, Windows and etc.). Here not lists how to install it.

Please refer to https://www.ruby-lang.org/en/downloads/ to install ruby on the platform you use.

If you are using Linux or Mac, I recommend you to install ruby through [RVM](http://www.rvm.io/) (Ruby Version Manager). 

"*RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems*."

Install RVM with a Ruby:

```bash
$ \curl -sSL https://get.rvm.io | bash -s stable
```

## Magic

```ruby
$ irb
> name="world"
=> "world"
> puts "Hello #{name}"
Hello world
=> nil
```

## Input and Output

```ruby
> name=gets
Jeoygin
=> "Jeoygin\n"
> puts "Hello #{name}"
Jeoygin
=> nil
> puts 'Hello world'
Hello world
=> nil
> print 'Hello world'
Hello world=> nil
> profession=$stdin.gets
Engineer
=> "Engineer\n"
> $stdout.puts "I'm #{profession}"
I'm Engineer
=> nil
> $stderr.puts "This is error info"
This is error info
=> nil
> inp = $stdin.read
First line
Second line
Third line
Last line
Ctrl+D
=> "First line\nSecond line\nThird line\nLast line\n"
> ios = IO.new STDOUT.fileno
=> #<IO:fd 1>
> ios.write "Jeoygin\n"
Jeoygin
=> 8
> ios.close
=> nil
```

**puts** append "\n" at the end automatically.

`redirect.rb`

```ruby
#!/usr/bin/ruby

$stdout = File.open "output.log", "a"

puts "Ruby"
puts "Java"

$stdout.close
$stdout = STDOUT

puts "Python"
```

## Useful Methods

```ruby
> 8.class
=> Fixnum
> Fixnum.superclass
=> Integer
> "Hello".length
=> 5
> "Hello".index("l")
=> 2
> nil.nil?
=> true
> 8.methods
=> [:to_s, :inspect, :-@, :+, :-, :*, :/, :div, :%, :modulo, ...
 :nil?, :=~, :protected_methods, :private_methods, :public_methods, 
 :instance_variables, :instance_of?, :is_a?, :equal?, :object_id, ...
 ]
```

## Control Structures

### Condition

```ruby
> x = 4
> if x > 10
    puts "Greater than 10"
  elsif x < 0
    puts "Less than 0"
  else
    puts "1..10"
  end
1..10
=> nil
> puts 'This appears to be true.' if x == 4
This appears to be true.
=> nil
> puts 'This appears to be false.' unless x == 4
=> nil
> unless x == 4
    puts 'This appears to be false.'
  else
    puts 'This appears to be true.'
  end
This appears to be true.
=> nil
```

### Loop

```ruby
> x = 0
> x = x + 1 while x < 10
=> nil
> x
=> 10
> x = x - 1 until x == 1
=> nil
> x
=> 1
> while x < 10
    x = x + 1
    puts x
  end
2
3
4
5
6
7
8
9
10
=> nil
```

### Function

```ruby
> def hello(name = 'world')
    puts "Hello #{name}"
  end
=> nil
> hello
Hello world
=> nil
> hello 'Jeoygin'
Hello Jeoygin
=> nil
```

## Collection

### Array

```ruby
> arr = [8, 'blue', 1.414]
=> [8, "blue", 1.414]
> arr[0]
=> 8
> arr[-1]
=> 1.414
> arr[0..2]
=> [8, "blue", 1.414]
> arr[0...2]
=> [8, "blue"]
> (0..1).class
=> Range
> arr.push((1..3).to_a)
=> [8, "blue", 1.414, [1, 2, 3]]
> arr.pop
=> [1, 2, 3]
> arr.pop
=> 1.414
> arr[1] = nil
=> nil
> arr
=> [8, nil]
> arr = %w[blue red green yellow]
=> ["blue", "red", "green", "yellow"]
> empty1 = []
=> []
> empty2 = Array.new
=> []
```

### Hash

```ruby
> user = {
    'name' => 'Jeoygin',
 	'age' => 'secret'
  }
=> {"name"=>"Jeoygin", "age"=>"secret"}
> user['name']
=> "Jeoygin"
> user['hobby']
=> nil
> dict = Hash.new(0)
=> {}
> dict['key1']
=> 0
> dict['key1'] = dict['key1'] + 1
=> 1
> dict[:array] = [1, 2, 3]
=> [1, 2, 3]
dict
=> {"key1"=>1, :array=>[1, 2, 3]}
```

## Variable and Symbol

|Local Var|Global Var|Instance Var|Class Var|Constants and Class Names|
|:-------:|:--------:|:----------:|:-------:|:-----------------------:|
|name     |$ARGS     |@name       |@@NUM    |NilClass                 |
|arraySize|$Global   |@credit_card|@@amount |PI                       |
|x_axis   |$config   |@AGE        |@@x_pos  |Test_Module              |

```ruby
> 'string'.object_id
=> 70149202218820
> 'string'.object_id
=> 70149198469560
> :string.class
=> Symbol
> :string.object_id
=> 158248
> :string.object_id
=> 158248
```

## Class and Module

```ruby
> 8.class
=> Fixnum
> 8.class.superclass
=> Integer
> 8.class.superclass.superclass
=> Numeric
> 8.class.superclass.superclass.superclass
=> Object
> 8.class.superclass.superclass.superclass.superclass
=> BasicObject
8.class.superclass.superclass.superclass.superclass.superclass
=> nil
> Fixnum.class
=> Class
> Class.class
=> Class
> Class.superclass
=> Module
> Class.superclass.superclass
=> Object
> Module.class
=> Class
```

### Class

```ruby
class User
  attr_accessor :first_name, :last_name
  
  def initialize(first_name, last_name)
    @first_name = first_name
	@last_name = last_name
  end
  
  def hello()
    puts "Hello #{first_name} #{last_name}"
  end
end
```

```ruby
> user = User.new('Jeoygin', 'Wang')
=> #<User:0x007f99c418a280 @first_name="Jeoygin", @last_name="Wang">
> user.hello
Hello Jeoygin Wang
=> nil
```

Ruby supports inheritance. But a class can only inherit from a single other class.

```ruby
> class Admin < User
> end
=> nil
> admin = Admin.new('jeoygin', 'Wang')
=> #<Admin:0x000000013236a0 @first_name="Jeoygin", @last_name="Wang">
> admin.hello
Hello Jeoygin Wang
=> nil
```

### Module

```ruby
module Hello
  def hi
    puts "Hello " + to_s
  end
end

class Person
  include Hello
  attr_accessor :name
  
  def initialize(name)
    @name = name
  end
  
  def to_s
    name
  end
end
```

```ruby
> Person.new('Jeoygin').hi
Hello Jeoygin
=> nil
```

## Block

```Ruby
> 3.times {puts "Hello"}
Hello
Hello
Hello
=> 3
> def threeTimes
    yield
    yield
    yield
  end
=> nil
> threeTimes {puts "Hello"}
Hello
Hello
Hello
=> nil
> def call_block(&block)
    block.call
  end
=> nil
> call_block {puts "Hello block"}
Hello block
=> nil
```

## Regular Expressions

```ruby
> line = 'Perl, zero or more other chars, then Python'
=> "Perl, zero or more other chars, then Python"
> puts "Scripting language mentioned: #{line}" if line =~ /Perl|python/
Scripting language mentioned: Perl, zero or more other chars, then Python
=> nil
> line.sub(/Perl/, 'Ruby')
=> "Ruby, zero or more other chars, then Python"
> 'Python, Python, Python'.gsub(/Python/, 'Ruby')
=> "Ruby, Ruby, Ruby"
```

## Meta Programming

### Open Class

```ruby
class NilClass
  def blank?
    true
  end
end

class String
  def blank?
    self.size == 0
  end
end
```

```ruby
> ["", "tiger", nil].each do |element|
    puts element unless element.blank?
  end
tiger
=> ["", "tiger", nil]
```

### method_missing

```ruby
class Echo
  def self.method_missing name, *args
    puts ([name] << args).join(' ')
  end
end
```

```ruby
> Echo.hello 'world'
hello world
=> nil
```

## Source Mirror

For Chinese developers, you know that.

```bash
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
```

## Reference

* [Programming Ruby: The Pragmaic Programmer’s Guide](http://ruby-doc.com/docs/ProgrammingRuby/)
* [七周七语言:理解多种编程范型](http://www.amazon.cn/%E4%B8%83%E5%91%A8%E4%B8%83%E8%AF%AD%E8%A8%80-%E7%90%86%E8%A7%A3%E5%A4%9A%E7%A7%8D%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B-%E6%B3%B0%E7%89%B9/dp/B008041DUY)
