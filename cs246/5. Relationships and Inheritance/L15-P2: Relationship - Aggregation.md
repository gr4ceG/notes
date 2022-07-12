## System Modelling
Visualize the structure of a system (abstractions and the relationship among them) to aid in design and implementation 

Popular standard UML class diagrams (Unified Modelling Language)

### Modelling a Class UML
...(1)

### Relationship: Composition of classes
```c++
class Vec {
    int x, y;
    public: 
    Vec(int x, int y): x{x}, y{y}{}
};
```

Two vectors define a basis: 
```c++
class Basis {
    Vec v1, v2;
};

Basis b; // Error 
// can't default construct v1 and v2
```
The built in default constructor for `Basis` tries to default construct `v1` and `v2`, no such `Vec::Vec()` constructor exists, so we can write our own that explicitly initializes them.

```c++
class Basis {
    Vec v1, v2;
    public:
        Basis(): v1{1, 0}, v2{0, 1}{}
        // invokes constructors on v1 and v2 - MUST BE DONE IN MIL - remember by time constructor body run fields are initaialized
}
```
Embedding objects (e.g `v1` and `v2`) within another (e.g `b`) is a *composition relationship*. 
> *Relationship*: a `basis` object "owns-a" `Vec` object (in fact it owns two)

If A "owns-a" B, then typically:
- B has no identity outside of A (no independent existence)
- If A is destroyed, then B is destroyed 
- If A is copied them B is copied (e.g a deep copy)

Composition is typically implemented through embedding objects within one another or through a pointer of which ownership.

...(2)

- The 2 says: "Each basis owns exactly 2 vecs"
- The 1 says: "Each vec is owned by exactly one basis"

### Relationship: Aggregation
> Students in a course, courses have students, but students have an independent existance outside of the course

This is a "has-a" (aggregation) relationship

If A "has-a" B, then typically:
- B exists apart from its association from it 
- If A is destroyed, B lives on
- If A is copied, B is not (shallow copy) - copies of A, share the same B

Example: parts of a catalogue

Modelling 
... (3)

Typical implementation of an aggreggation relationship is through a *non-owning pointer or reference* 

*e.g* 
```c++
class Catalogue {
    Part * parts[500];
    ...
    public:
        ~Catalogue() {// don't delete the parts}
};
```

### Relationship: Specialization
Suppose you want to track your collection of books 
```c++
class Book {
    string title, author;
    int length;
    public: 
        Book(...);
        ...
};
```

For textbooks, we also want to know the topic.
```c++
class Text {
    string title, author;
    int length;
    string topic;
    public:
        Text(...);
}
```
