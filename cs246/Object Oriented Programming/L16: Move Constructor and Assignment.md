# Move Constructor and Assignment 

We can write a Node move constructor as: 

```c++
struct Node {
    ...
    Node(Node && other): data{other.data}, next{other.next}
    {other.next = nullptr;}
    ...
}; 
```

Move assignment will be similar, but now need to dal with old data else leaks memory! Use the fact that the `rvalue` will be destroyed "soon"!

```c++
struct Node { 
    ...
    Node z operator = (Node && other) { // DO NOT set objects to nullptr here
        std::swap(data, other.data); 
        std::swap(next, other.next); 
        return *this; 
    }
}; 
```

## Move/Copy Elision 
As of c++11, compilers are required to perform move and copy elision and instead write the information directly into memory even if constructor does more (eg. print/incrementing a counter)
-> compile with `-fno-elide-constuctors`

`https://www.cplusplus.com/doc/tutorial/classes2`

| Member function | Implicitly defined: | Default definition |
|---|---|---|
| `Default constructor` | If no constructors | Does nothing |
| `Destructor` | If no destructor | Does nothing |
| `Copy constructor` | If no move constructor and no move assignment | Copies all members |
| `Copy assignment` | If no move constructor and no move assignment | Copies all members |
| `Move constructor` | If no destructor, copy constructor and no copy/move assignment | Moves all members |
| `Move assignment` | If no destructor, copy constructor and no copy/move assignment | Moves all members |

- If you provide copy operators but not move, compiler will use the copy operator 

### Rule of 5
If implement any one of: deconstructor, *move ctor, copy ctr, move =, copy =*, usually should implement all 

# Invariants and Encapsulation

*Example*: Assume Node class defined 
```c++
Node n1 {1, new Node{2}}; 
Node n2 {3};
Node n3 {4, &n2};  
```
When `n3` goes out of scope, the destructor attempts to free run-time memory: 

- Node class assumes the invariant that next is alwys either `nullptr` or heap address 
- Enforcing invariants requires the introduction of "encapsulaton" (*i.e* code is treated as capsules/black boxes) -> can only access through public interface 
- Encapsulation does not require OOP 

## Assessibility Modifiers 

Assessibility modifers: private, public (protected is only for inheritance)
- `struct` by default makes everything public, but can choose to override it 
```c++
struct Vec {
    Vec(int x, int y); // PUBLIC by default
    private: 
        int x, y; 
    public:
        Vec operator+(const Vec & rhs); 
}; 
```

## Classes 
-> introduce ==`class` keyword, where `private` is the default== 

```c++
class Vec {
    int x, y; // defaulted as PRIVATE
    public: 
        Vec(int x, int y); 
        Vec opertor+(...); 
}; 
```

```c++
#ifndef _LIST_H_
#define _LIST_H_

class List {
    class Node; 
    Node *theList = nullptr; 

    public:
    void addToFront(int n); 
    int ith(int i); 
    ~List(); 
}; 

#endif
```
- List now only provides limited interface; can't change contents directly
- But traversal of List is now `O(n^2)`
- If introduce iterator design pattern, can get this back to `O(n)` since an iterator is an abstraction of a pointer into the data structure 

1) Add to List: `begin()` which returns an iterator to first item past last item [Iterator is "nested" within List class]
2) Iterator: needs to be able to compare two Iterator objects: 
operator `==`, operator `!=`, 
operator `++` moves to "next" item in data structure, 
operator `*` dereferences the iterator and returns "data"

```c++
class List {
    struct Node; // forward declaration: there is going to be this type 
    Node *theList; 
    public: 
        class Iterator {
            Node * ptr; 
            public: 
                explicit Iterator(Node *p); 
                Iterator & operator++(); 
                int & operator(); 
                bool operator == (const Iterator&); 
                bol operator != (...); 
        }; 
        Iterator begin(); 
        Iterator end(); 

    List::Iterator::Iterator(Node *p): ptr{p}{}
    int & List::Iterator::operator *() {return p->data;} 
    List::Iterator & List::Iterator::operator ++() {
        ptr=ptr->next; 
        return *this; 
    }
    bool List::Iterator::operator == (Iterator &other) {
        return ptr == other.ptr; 
    }
    bool List::Iterator::opertor != (Iterator &other) {
        return !(*this == other); 
    }
    List::Iterator List::begin() {
        return List::Iterator {theList}; 
    }
    List::Iterator List::end(){
        return List::Iterator{nullptr}; 
    }
}; 
```