```
Student abdur{60, 70, 80}; 
Student caroline = abdur; // equivalently 
// Student caroline{60, 70, 80};
```

*How does Caroline's initialization happen?* via the copy constructor, which is invoked when one object is initialized with another of the same type.

Note: every class comes with (built-in): 

- A default constructor (default constructs all fields which are objects, lost if you define any constructor)
- A copy constructor (copy initializes all fields)
- A copy assignment operator (copy assigns all fields)
- A destructor 
- A move consructor 
- A move assignment operator 

# Constructors 

To build your own copy constructor: 

``` c++
Struct Student {
	int assns, mt, final; 
	Student (...) {...}
	Student (const Student &other): assns{other.assns}, mt {other.mt}, final{other.final}{} 
}; 
```

Generally `const` (Unless needed to change)
```bash 
Student (const Student &other)
```

Must be a **pass-by-reference**: `Student (const Student &other): ...`, if pass by value, and we called the copy constuctor, then we must call the copy constuctor to copy the value, which then must call the copy constructor (infinite loop). 

## Shallow Copy 

Consider: 

``` c++
Struct Node {
	int data; 
	Node *next; 
	Node(int data, Node *next): data{data}, next{next}{}
}; 

Node *n = new Node{1, new Node{2, new Node{3, nullptr}}}; 
Node m = *n; // copy constructor m with *n
Node *p = new Node{*n}; // copy constructor 
```

`n`, `m`, and `p` are on the stack, the data that `n`, `p`, and `m.next` point at is on the heap. The built in copy constructor just copy initializes all fields, each object has the same next pointer, so they share tails ==(shallow copy)==. 

```c++
m.next->data = 10; 
cout << p->next->data; // prints 10 
```

## Deep Copy 

If we want a deep copy (so each list is distinct copies of the values, changing the data of one tail doesn't affect the others), write your own copy constructor.

Consider: 

```c++
Struct Node {
	int data; 
	Node *next; 
	Node(const Node &other): data{other.data}, next{other.next? new Node{*other.next}; nullptr}; 
    // null - base case, must check that the next pointer NOT NULL before dereferencing (*other.next)
}
```

The copy constructor is called when: 

- An object is initialized by another object of the same
- When an object is passed by value (need to copy into function's stack frame)
- When an object is returned by value, (must copy to the caller's stack frame)

These statements are true for now, but we'll see excpections shortly

## Single Parameters, Implicit Conversions

**Note**: be careful with constructors that take a single parameter: 

```c++
Struct Node {
	...
	Node (int data): data{data}, next{nullptr}{}
}

Node n(4); calls the constructor as intended 
Node n = 4; // also calls it, implicit conversion
int f(Node n){...}
f(4); // works, compiler silently converts 4 to a Node 
```

Could accidentally pass the wrong argument to a function, (maybe we passed an int value when we meant to pass a node, would be nice to catch this error)

*Idea*: Disallow compiler from using this constructor for implict conversions by marking it `explicit` 

```c++
Struct Node {
	explicit Node(int data): data{data}, next{nullptr}{}
}; 

Node n(4); // explicitly calling constructor
Node n = 4; // ERROR
f(4); // ERROR
Node n = Node{4}; // OK
f(Node{4}); // OK
```
# Destructors 

When an object is destroyed (for stack-allocated objects when they go out of scope and heap-allocated objects when they're deleted), a method called the `destructor` runs, specifically the following sequence: 

1. Destructor body runs
2. Destuctors are invoked for fields which are objects 
3. Space is deallocated 

Classes come with a destructor (empty body) so just does nothing (phase 2 of object desctruction still runs)

When do we need to write our own?

```c++
Node *np = new Node{1, new Node{2, new Node{3, nullptr}}}; 
... 
delete np; // This ONLY deletes the node np directly points at, not the rest of the list
``` 

After `delete np`, we've leaked the rest of the list. 

We could iterate through the list, freeing each node ourselves, but that's not our job - that node OWNS its next node, it is responsible for cleaning it up (just like we own `np`, and are responsible for deleting it)

So, we write our own destructor: 

```c++
Struct Node {
	...
	~Node(){ delete next; }
}
```
Now: 
delete `np`; Frees the whole list, the node `np` points as is destroyed, as such its constructor is run, its destructor deletes its next, that is a Node object being destroyed, so ... etc. Until we reach reach `delete next`, where next is the nullptr, delete nullptr is a no-op (does nothing).