# List Iterators
Iterator uses efficiencies: 
- rather than always typing `List::Iterator it = ...;`, use "type deduction" via `auto` keyword: 
```c++
auto it = ...; // figures out type from RHS
```
Uses *duck typing* -> dynamic typing ...

The iterator class is nested in the list class `List::Iterator`: `::` tells us the scope of `Iterator` is `List`

- rather than always writig out loop iteration code, use "range-based" for loop: 
```c++
for (List::Iterator it = lst.begin(); it != lst.end(); ++it) {
    cout << *it << endl; 
}

for (auto n: lst) { cout << n << endl; } // can't change the list (1.e copy)
for (auto& n: lst) { n+= 5 } // & - changes list contents  
```
## Modifying List Iterator Assessibility 
`List::Iterator` constructor is currently public, so *anybody* can create one: 
```c++
List::Iterator it{ nulltptr }; 
```
Would prefer that the client be *required* to use `List::begin()` or `List::end()`: 
1. make `List::Iterator` constructor private
2. make List a "friend" 
   - A friend class/method/function can see *everything* (use with care!): In `List::Iterator` class definition, make `List` a friend 
```c++
class List {
    ...
    class Iterator {
        ...
        friend class List; // List is a friend to Iterator: can access private constructors 
    }
}
```
### Q. How do we acess or change data in a class?
A: provide "accessors" and/or "mutators", which access and mutates data respectively 
- Public methods to control access/change
- Assessors probably should not return by reference, but `const` reference is okay

## Member Operators
Some operators must be part of the class: 
- operator `=`: assignment
- operator `[]`: index 
- operator `->`
- operator `()`
- operator `T()`, where `T` is a type 

Remember that implicitly "`this`" (object address) is the *first* parameter of any *non-static* class method: 
```c++
class Vec {
    int x, y; 
    public: 
        Vec(int x = 0, int y = 0); 
        Vec operator+(const vec& other); // the `this` that gets generated is the address of v1
        ...
}

Vec v1, v2; int i = 5; 
v1 + v2; // v1.operator+(v2) -> !Observe note below
v1 + i; 
```
!Note: when calling `.operator+`, the `this` that gets generated (implicitly) is the address of `v1`, and other (passed argument) is the address of `v2`

- If the first parameter of (non-static) method *isn't* the class type, then it has to be declared outside the class: 
```c++
Vec Vec::operator*(int i); // v1 * 2
Vec operator*(const Vec& v, int i); 

Vec operator*(int i; const Vec& v); // 2 * v1
// first parameter isn't the class type, hence must be declared outside the class 
// CANNOT DO: 
Vec Vec::operator*(const Vec& v); // Cannot assume "implicit" this for the int (The class is Vec)
```
- I/O operators `(<<, >>)` *must* be outside the class since the first parameter is a stream object

# Object Arrays 
- Often want to create arrays of objects: 
```c++
Student course [100]; // only works is Student has a DEFAULT CONSTRUCTOR
```
- Can specify individual constructor calls: 
```c++
Vec varr[3] = { Vec{0, 0}, Vec{1, 2}, Vec{-1, 13}}; 
```
- Can make individual constructor calls as allocate each object on the heap (array may or may not be on the heap)
```c++
Vec *arr1[100]; 
for (int i = 0; i < 100; ++i) { // Array is on the STACK 
    arr1[i] = new Vec{i, i + 1}; 
}
...
for (...) delete arr1[i]; 
```
```c++
Vec **arr2; // Array is on the HEAP
arr2 = new Vec *[50]; 
for (...) {
    arr2[i] = new Vec{i, i + 1}; 
}
...
for (...) delete arr2[i]; 
delete arr2[]; 
```

# Constant Objects
```c++
void foo(const Vec& v) {...}
```
Can't call `v.xxx();` unless it is guaranteed to the compiler that `vec::xxx()` *won't* change the object's contents 
```c++
void xxx() const; // `const` must be repeated on implementation signature
```