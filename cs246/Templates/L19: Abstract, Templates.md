<style>
r { color: #F95050 }
navy { color: #000099 }
b { color: #50C1F9 }
o { color: #F7A63B }
g { color: #32a852 }
</style>

```c++
class Book {
    clss Comic : public Book { //(override????)
        return numPages > 200;
    }
}
```
This code executes `Book::isHeavy` for books, `Text::isHeavy` for texts, and `Comic::isHeavy` for comics.

The code accomodates multiples types under one abstraction (this book abstraction) - **polymorphism ("many forms")**

Note: this is why out input operator takes on `istream&` but can operator on `ifstream`, `istringstream`, etc.

## Destructor Revisited
```c++
class X {
    int *x; 
    public:
        x(int n): x{new int{n}}{}
        ~x(){delete x;}
}; 

class Y : public X {
    int *y;
    public:
        Y(int m, int n): x{n}, y{new int{m}}{}
        ~Y(){delete Y;}
}

x *myX = new Y{10, 20};
...
delete myX;
```
When destrucor is not virtual, this only calls `X::~X()`, if it is virtual, then `Y::~X()` is called, as part of destroying that Y, its superclass component is destroued and X's destructor is run as well.

<r>If you have inheritance, your destructor should always be virtual</r>, even if the base class doesn't have resources to manage, the derived classes later might.

## Abstract Classes

```c++
class Student { // base class/parent class/superclass
    ...
    protected:
        int numCourses;
    public:
        virtual int fees() const = 0;
        ...
};
```

There are two kinds of students: regular and coop.
```c++
class Regular: public Student {
    ...
    public:
        int fees() const override; // code to compute fees for regular student
}

class Coop: public Student {
    public:
        int fees() const override;// code to compute fees for coop student
}
```
But - what should we put for `Student::fees()`? Not sure - every student should be regular or coop: there should be no student objects.

Here, `Student::fees()` is a <r>**pure virtual method**</r>, which is a method that <r>*does not require and implementation*</r>. ==A class with a pure virtual method is called *abstract* and cannot be instatiated==.

```c++
Student s; //ERROR - Student is abstract
```
The purpose of abstract classes is to define common interfaces and organize subclasses. Subclasses of an abstract class are *also abstract if the **do not override** the pure virtual methods*.

Non-abstract classes are called concrete. 

In UML, visualize abstrcat classes and pure virtual methods by *italicizing* their name. 

## Templates 
```c++
class List {
    struct Node: 
    Node *theList; 
    ...
};
```
What if we wanted to store something other than `int`s in our List?

A template class is a class paramaterized by a type. 
```c++
template <typename T>
class Stack {
    int size;
    int cap;
    T *contents;
    public:
        Stack() {...};
        void push(T x) {...};
        T top() {...}; 
        void pop{...}
};
```
We rewrite List as a template class
```c++
template <typename T>
class List {
    struct Node {
        T data;
        Node *next;
    }
    public:
        class Iterator {
            Node *p;
            ...
            public:
                ...
                T &operator() {return p->data};
                ...
        };
        void addTofront(T x) {
            theList = new Node{x, theList};
        }    
};

List<int> l1;
List<char> l2;
List <List<int>> l3;

l1.addToFront(1);
l3.addToFront(l1);

for (List<List<int>>::iterator it = l3.begin(); it!=l3.end; ++it) {
    for (auto n : *it) {
        cout << n << " ";
    }
    cout << endl;
}
```
How do templates work? Roughly speaking the compiler sees each instantiation of the class (e.g `List<int>`) and generates specialized classes for each type declared.

### The Standard Template Library (STL)
Large collection of useful templates
Example: dynamic-length arrays
```c++
#include <vector>
using namespace std;

vector<int> v{4,5}; // vector containing (4, 5)
vector<int> w(4,5); // vector containing 4 5's (5,5,5,5)

v.emplace_back(6); // {4,5,6}
v.emplace_back(7); // {4,5,6,7}
```

```c++
vector<Book> b;
b.push_back(Book{"Title", "author", 75}); // construct temporarily, movees it in the back of vector
b.emplace_back("Title", "author", 75); // constructs the object directly in the back of the vector
```
> Note: Can use an array of pointers polymorphically, cannot use an array polymorphically: adding a derived class object will slice it into a base class object

```c++
{
    Vector<int> v{3,2,1};
} // v goes out of scope, array is freed and nothing is leaked
```
But!
```c++
{
    Vector<int*> w{new int{1}, new int{2}};
    for (auto &p : w) delete p;
}
```
Looping over vectors:
```c++
for (int i=0; i < v.size(); ++i) {
    cout << v[i] << endl;
}
```
Vectors also support the iterator abstraction!
```c++
for (vector<int>::iterator it=v.begin(); it !=v.end(); ++it) {
    cout << *it << endl;
}
\\ OR INDEED
for (auto m : v){cout << m << endl;}
```
To iterate in reverse:
```c++
for (vector<int>::reverse_iterator it=v.rbegin(); it!=rendl(); ++it) { ... }
v.pop_back(); // removes the last element of V
```
Vectors are guarenteed to be implemented as arrays internally. Now that you know about vectors, you should use them whenever you need a dynamic array length array. (i.e you shouldn't need to use the array forms of `new` and `delete`)

Many other vector operations are based on iterators (i.e `erase` method which takes an iterator to the element you want to remove)

> Iterator invalidation! Be careful w/ iterators and realocating memory!! once reallocated memory, the iterator no longer points as the right element (points to element in old array/vector)

