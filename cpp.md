# Learn C++ in X Minutes

C++ is a middle-level programming language developed by Bjarne Stroustrup starting in 1979 at Bell Labs. C++ runs on a variety of platforms, such as Windows, Mac OS, and the various versions of UNIX.

## Hello World

```cpp
#include <iostream>

using namespace std;

int main() {
   cout << "Hello World" << endl;
   return 0;
}
```

```bash
$ g++ -o hello hello.cpp
$ ./hello
Hello World
```

## Basics

### Default Values for Function Parameters

```cpp
#include <iostream>
using namespace std;
 
int mul(int a, int b = 1) {
  return a * b;
}

int main () {
   cout << "Result is :" << mul(3, 4) << endl;
   cout << "Result is :" << mul(5) << endl;
 
   return 0;
}
```

### References

A reference is an alias of another existing variable. Either the variable or the reference name may be used to refer to the variable.

There are three major differences between references and pointers:

* A reference must be initialized when it is created.
* References can't be NULL. A reference must be connected to a legal member storage.
* Once a reference is initialized, it can't be changed to refer to another variable.

```cpp
#include <iostream>
using namespace std;

int main() {
    int i;
    double d;
    int& ri = i;
    double& rd = d;

    i = 88;
    cout << "i: " << i << endl;
    cout << "i reference: " << ri << endl;

    d = 3.14;
    cout << "d: " << d << endl;
    cout << "d reference: " << rd << endl;

    return 0;
}
```

## Classes and Objects

### Class Definitions

```cpp
class Box {
   public:
      double length;
      double breadth;
      double height;
};
```

**Create Objects:**

```cpp
Box box1;
Box box2;
```

**Access Object Members:**

```cpp
double volumn = box1.height * box1.length * box1.breadth;
```

### Class Member Functions

```cpp
class Box {
   public:
      double length;
      double breadth;
      double height;
      double getVolume(void);
};

double Box::getVolume(void) {
    return length * breadth * height;
}
```

or

```cpp
class Box {
   public:
      double length;
      double breadth;
      double height;
   
      double getVolume(void) {
         return length * breadth * height;
      }
};
```

### Class Access Modifiers

```cpp
class Base {
   public:
       // public members go here
 
   protected:
       // protected members go here
 
   private:
       // private members go here
};
```

The default access modifier for class and member is private.

* A public member is accessible from anywhere outside the class but within a program.
* A private member cannot be accessed from outside the class.
* A protected member is similar to a private member but can be accessed in child classes.

### Constructor and Destructor

#### Constructor

```cpp
class Box {
    public:
        Box(); // This is the constructor
};

// Constructor definition
Box::Box(void) {

}
```

**Parameterized constructor:**

```cpp
class Box {
    public:
        Box(double len); // This is the constructor

    private:
        double length;
};

// Constructor definition
Box::Box(double len) {
    length = len;
}
```

**Initialize fields using initialization lists:**

```cpp
Box::Blox(double len): length(len) {
}
```

#### Destructor

```cpp
class Box {
    public:
        Box();    // This is the constructor
        ~Box();   // This is the destructor

    private:
        double length;
};

Box::Box(void) {
}

Box::~Box(void) {
}
```

### Copy Constructor

The **copy constructor** is a constructor that creates an object by initializing it with an object of the same class.

```cpp
classname (const classname &obj) {

}
```

```cpp
class Box {
    public:
        int getLength(void);
        Box(int len);         // constructor
        Box(const Box &obj);  // copy constructor
        ~Box();               // destructor

    private:
        int *ptr;
};

Box::Box(int len) {
    ptr = new int;
    *ptr = len;
}

Box::Box(const Box &obj) {
    ptr = new int;
    *ptr = *obj.ptr;
}

Box::~Box(void) {
    delete ptr;
}

int Box::getLength(void) {
    return * ptr;
}

int main() {
    Box box1(10);
    Box box2 = box1; // this calls copy constructor

    return 0;
}
```

### Friend Functions

```cpp
#include <iostream>
 
using namespace std;
 
class Box {
    public:
        friend void printWidth( Box box );
        void setWidth( double wid );

    private:
        double width;
};

void Box::setWidth(double wid) {
    width = wid;
}

// This is not a member function of any class.
void printWidth(Box box) {
   cout << "Width of box : " << box.width <<endl;
}
 
int main( ) {
   Box box;
   box.setWidth(10.0);
   printWidth( box );
   return 0;
}
```

### Inline Functions

If a function is inline, the compiler replace the function invocation with a copy of the function at every point where the function is called.

```cpp
inline int Max(int x, int y) {
    return (x > y) ? x : y;
}
```

### Pointers

#### this

```cpp
#include <iostream>

using namespace std;

class Box {
    public:
        int compare(Box box);
        Box(int length);

    private:
        int length;
};

Box::Box(int length) {
    this->length = length;
}

int Box::compare(Box box) {
    return this->length - box.length;
}

int main() {
    Box box1(2);
    Box box2(1);

    cout << "box1 - box2 = " << box1.compare(box2) << endl;

    return 0;
}
```

#### Pointer to classes

```cpp
int main() {
    Box box1(2);
    Box *pBox1 = &box1;
    Box *pBox2 = new Box(1);

    cout << "box1 - box2 = " << pBox1->compare(*pBox2) << endl;

    return 0;
}
```

### Static Members

```cpp
#include <iostream>

using namespace std;

class Box {
    public:
        static int objectCount;
        Box(int length);
        static int getCount();

    private:
        int length;
};

int Box::objectCount = 0;

Box::Box(int length) {
    this->length = length;
    objectCount++;
}

int Box::getCount() {
    return objectCount;
}

int main() {
    cout << "Initial Count: " << Box::getCount() << endl;

    Box box1(2);
    Box box2(1);

    cout << "Final Count: " << Box::getCount() << endl;

    return 0;
}
```

## Inheritance

```cpp
class derived-class: access-specifier base-class
```

The access modifier of members of the base class = min( class-access, member-access ), private &lt; protected &lt; public.

### Multiple Inheritances

```cpp
class derived-class: access baseA, access baseB...
```

## Overloading

### Function overloading

```
class Adder {
    public:
        int add(int a, int b) {
            return a + b;
        }

        double add(double a, double b) {
            return a + b;
        }
};
```

### Operators overloading

For member functions:

```cpp
Box operator+(const Box&);
```

For non-member functions:

```cpp
Box operator+(const Box&, const Box&);
```

```cpp
class Box {
    public:
        void setLength(int length) {
            this->length = length;
        }

        Box operator+(const Box& b) {
            Box box;
            box.length = this->length + b.length;
            return box;
        }

    private:
        int length;
}
```

## Polymorphism

```cpp
#include <iostream> 
using namespace std;
 
class Shape {
   protected:
      int width, height;

   public:
      Shape(int a=0, int b=0) {
         width = a;
         height = b;
      }
      int area() {
         cout << "Shape area" <<endl;
         return 0;
      }
};

class Rectangle: public Shape {
   public:
      Rectangle(int a=0, int b=0): Shape(a, b) { }
      int area () {
         cout << "Rectangle area" << endl;
         return (width * height); 
      }
};

class Triangle: public Shape{
   public:
      Triangle(int a=0, int b=0): Shape(a, b) { }
      int area () { 
         cout << "Triangle area" <<endl;
         return (width * height / 2); 
      }
};

int main() {
   Shape *shape;
   Rectangle rec(3, 4);
   Triangle  tri(3, 4);

   shape = &rec;
   shape->area(); // Parent area

   shape = &tri;
   shape->area(); // Parent area
   
   return 0;
}
```

### Virtual Function

```cpp
class Shape {
   protected:
      int width, height;

   public:
      Shape(int a=0, int b=0) {
         width = a;
         height = b;
      }
      virtual int area() {
         cout << "Shape area" <<endl;
         return 0;
      }
};
```

The output result becomes:

```
Rectangle area
Triangle area
```

### Pure Virtual Function

```cpp
class Shape {
   protected:
      int width, height;

   public:
      Shape(int a=0, int b=0) {
         width = a;
         height = b;
      }
      virtual int area() = 0;
};
```

## Reference

* [Tutorialspoint C++ Tutorial](http://www.tutorialspoint.com/cplusplus/index.htm)