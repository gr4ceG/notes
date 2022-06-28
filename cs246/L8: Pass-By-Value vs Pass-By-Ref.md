# Pass-By-Value vs Pass-By-Ref
``` c++
int f(int n) {....}
// copies the argument, so if the arg is big, could be expensive

struct ReallyBig {...}

int f(ReallyBig rb ) {...} //pass by value, copies the arg, (potentially) slow

int g(ReallyBig &rb) {...};
// pass by reference, alias, fast, but could change rb in the caller

int h(const ReallyBig &rb) {...}
// pass by reference, fast, no copy, no risk of mutation
```

*Advice:* prefer pass by const ref over pass by value for anything larger than an int. Unless you needs to make a copy anyways, then pass by value

*Example*: make a copy

``` c++
int z(const ReallyBig &rb) {
	ReallyBig rc{rb};
	// .. do stuff with rc
}

int z(ReallyBig rc) {
	...
}

```

- Anything you can do with a pointer, you can do with a const ref
- **Streams can’t be copied, they can only be passed by reference**

*Example*:

``` c++
int f(int &n) {...}
int g(const int &n) {...}

f(5);
// would not work, since you cant initilize an lvalue reference to a literal value
// if n changes, you cannot change the literal value 5

g(5);
// would work, since we promise the compiler g will not change its paramter 
// how can g(5) work, if n must be bound to something with a memory address?
//the compiler puts the 5 in a temp location, so that the refernce n has something to point at

// thats why pass-by-const-ref is good for anything larger than a pointer
```

# Dynamic Memory Alllocation

in C:

``` c
int *p = malloc (... *sizeof (int)); 
...
free(p)
```

- Don't use this in C++
- Instead: `new`/`delete`
- They are type-aware (don't need to use `sizeof()`, less error-prone)

*Example:*

``` c++
struct Node {
	int data;
	Node *next;
};
Node *np = new Node; // *new* allocates Node on heap
...
delete np;
```

Note: 
 - `np` is in the stack, pointing up to the `Node` struct in the heap
 - all local variables are on the stack
 - variables are deallocated when they go out of scope (stack is popped)
 - Allocated memory exists on the heap
 - Remains allocated until `delete` is called
 - If you don’t delete all allocated memory – memory leak
 - Program will fail eventually – we regard any memory leaks in this class as an error
 - best way to check for memory leaks, use the tool **valgrind** on your program
	- eg. if your program is called prog (in your `cwd`): ***`valgrind ./prog`***

```C++
int foo (int x) {
	int *p = new int;
	*p = x + 1;
	return *p;
	// copy out memory address to caller stack frame
	// memory leak!
}

int main() {
	int n;
	cin >> n;
	while (n>0) {
		cout << foo(n) << endl;
		--n;
	}
}
```

```C++
int &foo(int x) {
	int n = x + 1;
	return n; //ERROR. returning n (created in this func)
}

int *bar(int x) {
	int n = x + 1;
	return &n; //ERROR
}

int fib(int n) {
	if (n==0 | n==1) return 1;
	
}

int main() {
	int &p foo(10);
	cout << p << endl;
}

```

## Allocating Arrays on the Heap
```c++
Node *myNode = new Node [10]; // Creates an array of 10 nodes
...
delete [] myNode; // delete the whole array, not the nodes: 
```

- Memory allocated with `new` must be deallocated with `delete`
- Memory allocated with `new [ __ ]` must be deallocated with `delete []`
- To mix these two would be ***undefined behaviour***

## Returning by Value/Pointer/Reference

``` C++
Node getMeANode() { 
	Node n;
	return n;
}
// possibily expensive – n is copied to the caller’s stack frame 
// if expensive, could we return a pointer or a reference instead?

Node *getMeANode() {
	Node n;
	return &n;
}
// BAD! returns a ptr to stack allocated data that ceases to be allocated when this function returns
// n is popped off the stack after the function finishes, 
// so you return a dangling reference 
// Program does not fail – has undefined behaviour but doesn’t crash

Node *getMeANode() {
	Node *np = new Node;
	return np;
}
// OK, returns a pointer to heap-allocated data which still exists after the function ends
// caller needs to delete this heap data 

Which should you pick? 
// You should pick return by value (first one)
	// unless you actually want heap allocated data) because it won't be as slow necessarily
// Sometimes the cost of a copy is better than its alternatives

```

## Operator Overloading

Give new meanings to C++ operators for our own types
```C++
struct vec {
	int x, y;
};

vec operator +(const vec &v1, const vec &v2) {
	vec v{v1.x + v2.x, v1.y + v2.y};
	return v;
}

vec operator *(const int k, const vec &v) { // implements k * v
	return {k*v.x, k*v.y}; // Ok – Complier knows you’re creating a vec based on return type
}

// you can do
vec a{1,2};
vec b{3,5};
vec c = a+b;
vec d = 5*a;

vec e = d*2; //not defined .. yet

vec operator *(const vec &v, const int k) { // implements v * k 
	return k*v; // uses the * operator above
}

// this is operator overloading
```

## Special Case: Overloading I/O Operator (`<<`, `>>`) 

*Example*: prints out grades
```C++
// Create a new type grades:
struct Grade {
	int theGrade;
};

// Want to print grades with a %, so overload the output operator:

// doesnt print endline, user might want to print more than one per line

ostream &operator<<(ostream &out, const Grade &g) { 				
	return out << g.theGrade << "%";
}

// Read in a grade, can just read in a number
istream &operator>>(istream &in, Grade &g) {
	in >> g.theGrade; // changes g’s grade to the number read in 
	if (g.theGrade < 0) g.theGrade = 0;
	if (g.theGrade > 100) g.theGrade = 100;

	return in; // because input operators also return their input value – for cascading
}